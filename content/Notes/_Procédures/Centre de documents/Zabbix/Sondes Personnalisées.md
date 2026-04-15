---
tags:
  - zabbix
  - monitoring
  - sonde
---

# Sondes Personnalisées Zabbix

## Théorie

### Qu'est-ce qu'une sonde personnalisée ?

Une **sonde personnalisée** (ou *UserParameter*) permet d'étendre les capacités de l'agent Zabbix en définissant ses propres métriques. Là où Zabbix collecte nativement des données système courantes (CPU, RAM, disque…), les UserParameters permettent de collecter **n'importe quelle valeur** : résultat d'un script, sortie d'une commande, valeur issue d'un outil tiers.

### Mécanisme de fonctionnement

```
Serveur Zabbix ──(requête clé)──► Agent Zabbix ──► Exécute commande/script ──► Retourne valeur
```

1. Le serveur Zabbix demande la valeur d'un **item** identifié par une **clé**
2. L'agent reconnaît la clé car elle est déclarée dans sa configuration
3. L'agent exécute la commande associée
4. La valeur (texte, entier, flottant) est renvoyée au serveur

### Déclaration d'un UserParameter

Dans le fichier de configuration de l'agent (`/etc/zabbix/zabbix_agent2.conf` ou un fichier dans `/etc/zabbix/zabbix_agent2.d/`), on ajoute :

```ini
UserParameter=ma.cle[*],/chemin/vers/script.sh "$1"
```

| Élément | Description |
|---|---|
| `ma.cle` | Nom de la clé utilisé dans l'interface Zabbix |
| `[*]` | Paramètres optionnels passables depuis l'item |
| `$1`, `$2`… | Arguments reçus depuis l'item Zabbix |

> [!important]+ Règles de sécurité
> - L'agent Zabbix s'exécute avec des droits limités (utilisateur `zabbix`)
> - Les scripts doivent être **lisibles et exécutables** par cet utilisateur
> - Ne jamais exposer de données sensibles dans la sortie

### Types de valeurs retournées

| Type Zabbix | Description | Exemple |
|---|---|---|
| `Numeric (unsigned)` | Entier positif | `42` |
| `Numeric (float)` | Décimal | `38.5` |
| `Character` | Chaîne courte (≤255 chars) | `OK` |
| `Text` | Texte long | Logs |

### Étapes de création complète

1. **Écrire le script** ou la commande
2. **Déclarer le UserParameter** dans la config de l'agent
3. **Redémarrer l'agent** : `systemctl restart zabbix-agent2`
4. **Tester localement** : `zabbix_agent2 -t ma.cle`
5. **Créer l'item** dans l'interface Zabbix (Configuration > Hôtes > Items)
6. Optionnel : créer un **trigger** et/ou un **graphique**

---

## Exemple 1 — Température du CPU

### Avec OpenHardwareMonitor (Windows)

**OpenHardwareMonitor** expose les données matérielles (températures, vitesses de ventilateur, tensions) via **WMI**, ce qui les rend accessibles sans droits élevés depuis un script.

#### Prérequis

- [OpenHardwareMonitor](https://openhardwaremonitor.org/) installé et lancé en tant que service (ou au démarrage)
- PowerShell disponible sur la machine Windows

#### Script PowerShell

Créer `C:\zabbix\scripts\cpu_temp.ps1` :

```powershell
$sensor = Get-WmiObject -Namespace "root\OpenHardwareMonitor" `
    -Class Sensor `
    | Where-Object { $_.SensorType -eq "Load" -and $_.Name -like "*CPU*" } `
    | Select-Object -First 1

if ($null -eq $sensor) { exit 1 }

$charge = [float]$sensor.Value
Write-Host ([math]::Round($charge, 1).ToString([System.Globalization.CultureInfo]::InvariantCulture))
```

#### Déclaration du UserParameter

Dans `C:\Program Files\Zabbix Agent 2\zabbix_agent2.conf` (ou un fichier `.conf` dans le dossier `Include`) :

```ini
UserParameter=cpu.temperature.ohm,powershell -NoProfile -ExecutionPolicy Bypass -File "C:\zabbix\scripts\cpu_temp.ps1"
```

#### Test local

```powershell
& "C:\Program Files\Zabbix Agent 2\zabbix_agent2.exe" -t cpu.temperature.ohm
```

Résultat attendu :
```
cpu.temperature.ohm                           [s|52.0]
```

---

### Sans OpenHardwareMonitor (Linux — `sensors`)

Sur Linux, l'outil `lm-sensors` expose les températures via la commande `sensors`.

#### Prérequis

```bash
apt install lm-sensors -y
sensors-detect --auto
```

#### Script Bash

Créer `/etc/zabbix/scripts/cpu_temp.sh` :

```bash
#!/bin/bash
# Récupère la température du premier capteur CPU trouvé
sensors | grep -E "^(Core 0|Tdie|Tccd1|Package)" | head -1 | awk '{print $NF}' | tr -d '+°C'
```

Rendre exécutable :

```bash
chmod +x /etc/zabbix/scripts/cpu_temp.sh
chown zabbix:zabbix /etc/zabbix/scripts/cpu_temp.sh
```

#### Déclaration du UserParameter

Dans `/etc/zabbix/zabbix_agent2.d/cpu_temp.conf` :

```ini
UserParameter=cpu.temperature,/etc/zabbix/scripts/cpu_temp.sh
```

#### Redémarrage et test

```bash
systemctl restart zabbix-agent2
zabbix_agent2 -t cpu.temperature
```

Résultat attendu :
```
cpu.temperature                               [s|49.0]
```

#### Item dans Zabbix

| Champ | Valeur |
|---|---|
| Nom | `Température CPU` |
| Type | `Zabbix agent` |
| Clé | `cpu.temperature` |
| Type d'information | `Numeric (float)` |
| Unités | `°C` |
| Intervalle de mise à jour | `60s` |

#### Trigger exemple

```
last(/Nom_hôte/cpu.temperature) > 80
```
Gravité : **High** — Message : `Température CPU critique : {ITEM.VALUE}°C`

---

## Exemple 2 — Débit réseau d'un port Ethernet

### Principe

Zabbix possède nativement des clés pour les interfaces réseau, mais celles-ci retournent des **compteurs cumulatifs d'octets** (toujours croissants). Pour obtenir un **débit en bits/s**, on utilise deux approches :

- **Clé native Zabbix** : `net.if.in[eth0]` / `net.if.out[eth0]` + calculer le delta via un **item calculé** ou le préprocesseur `Change per second`
- **UserParameter personnalisé** : lire directement `/proc/net/dev` ou `ip -s link`

### Approche recommandée — Clés natives + préprocesseur

#### Items à créer

**Item 1 — Octets reçus (brut)**

| Champ | Valeur |
|---|---|
| Nom | `eth0 - Octets reçus (compteur)` |
| Clé | `net.if.in[eth0,bytes]` |
| Type d'information | `Numeric (unsigned)` |
| Unités | `B` |
| Intervalle | `30s` |
| Préprocesseur | `Change per second` → résultat en B/s |

**Item 2 — Octets envoyés (brut)**

| Champ | Valeur |
|---|---|
| Nom | `eth0 - Octets envoyés (compteur)` |
| Clé | `net.if.out[eth0,bytes]` |
| Type d'information | `Numeric (unsigned)` |
| Unités | `B` |
| Intervalle | `30s` |
| Préprocesseur | `Change per second` → résultat en B/s |

> [!tip]+ Conversion en bits/s
> Ajouter un second préprocesseur **Custom multiplier** avec la valeur `8` pour obtenir des **bits/s** plutôt que des octets/s. Mettre l'unité à `bps`.

#### Trigger exemple

```
last(/Nom_hôte/net.if.in[eth0,bytes]) > 900000000
```
Gravité : **Warning** — Message : `Débit entrant eth0 > 900 Mbps`

---

### Approche alternative — UserParameter via `/proc/net/dev`

Utile si l'on veut un contrôle total ou des calculs personnalisés (ex. : débit en Mbps arrondi).

#### Script Bash

Créer `/etc/zabbix/scripts/net_bw.sh` :

```bash
#!/bin/bash
# Usage: net_bw.sh <interface> <direction>
# direction: in | out
IFACE="$1"
DIR="$2"

if [ "$DIR" = "in" ]; then
    COL=2   # colonne bytes reçus dans /proc/net/dev
else
    COL=10  # colonne bytes envoyés
fi

BYTES1=$(awk "/$IFACE:/{print \$$COL}" /proc/net/dev)
sleep 1
BYTES2=$(awk "/$IFACE:/{print \$$COL}" /proc/net/dev)

BPS=$(( (BYTES2 - BYTES1) * 8 ))
echo $BPS
```

Rendre exécutable :

```bash
chmod +x /etc/zabbix/scripts/net_bw.sh
chown zabbix:zabbix /etc/zabbix/scripts/net_bw.sh
```

> [!warning]+ Performance
> Ce script dure **1 seconde** à chaque exécution (à cause du `sleep 1`). Préférer les clés natives avec le préprocesseur `Change per second` pour de meilleures performances.

#### Déclaration du UserParameter

Dans `/etc/zabbix/zabbix_agent2.d/net_bw.conf` :

```ini
UserParameter=net.bw[*],/etc/zabbix/scripts/net_bw.sh "$1" "$2"
```

#### Test local

```bash
zabbix_agent2 -t "net.bw[eth0,in]"
zabbix_agent2 -t "net.bw[eth0,out]"
```

#### Items dans Zabbix

| Champ | Débit entrant | Débit sortant |
|---|---|---|
| Nom | `eth0 - Débit entrant` | `eth0 - Débit sortant` |
| Clé | `net.bw[eth0,in]` | `net.bw[eth0,out]` |
| Type d'information | `Numeric (unsigned)` | `Numeric (unsigned)` |
| Unités | `bps` | `bps` |
| Intervalle | `60s` | `60s` |

---

## Récapitulatif des commandes utiles

```bash
# Tester un UserParameter depuis la machine agent
zabbix_agent2 -t "ma.cle[param1,param2]"

# Vérifier la configuration de l'agent (syntaxe)
zabbix_agent2 -T

# Voir les logs de l'agent
journalctl -u zabbix-agent2 -f

# Lister les interfaces réseau et leurs compteurs
cat /proc/net/dev

# Lister les capteurs de température
sensors
```
