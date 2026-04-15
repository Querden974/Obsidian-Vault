# SNMP Cisco CBS 250 → Zabbix

## Prérequis

- Switch **Cisco CBS 250** accessible sur le réseau
- Serveur **Zabbix** installé et opérationnel (voir [[Installation serveur Zabbix]])
- Accès à l'interface web du CBS 250 (HTTP/HTTPS)
- UDP port **161** ouvert entre le serveur Zabbix et le switch

---

## Étape 1 — Activer et configurer SNMP sur le CBS 250

### a. Accéder à l'interface web du switch

Ouvrir un navigateur et aller sur l'IP du switch :
`http://<IP_DU_SWITCH>`

Se connecter avec les identifiants administrateur (par défaut : `cisco / cisco`).

### b. Activer SNMP

Passer en interface `advanced`

Aller dans `Security > TCP/UDP Services`

Cocher `SNMP Service`



Cliquer sur **Apply**.

### c. Configurer la communauté SNMP (SNMPv2c)

Aller dans `SNMP > Communities`

Cliquer sur **Add** et renseigner :

| Paramètre | Valeur |
|---|---|
| Community String | `public` *(ou une valeur personnalisée)* |
| Access Mode | **Read Only** |
| View Name | `Default` |

> [!warning] Ne pas laisser la communauté `public` en production. Utiliser une chaîne personnalisée.

Cliquer sur **Apply**.

### d. Restreindre les hôtes autorisés (recommandé)

Aller dans `SNMP > Access Control`

Ajouter l'IP du serveur Zabbix comme seul hôte autorisé à interroger le switch.


### e. Activer via CLI
```
snmp-server server
snmp-server community Superv_TSSR ro 192.168.255.75 view Default
snmp-server host 192.168.255.75 traps version 2c Superv_TSSR
```

---

## Étape 2 — Vérifier la communication SNMP depuis le serveur Zabbix

Depuis le serveur Zabbix (Linux), tester la connectivité SNMP :

```bash
snmpwalk -v2c -c public <IP_DU_SWITCH> 1.3.6.1.2.1.1
```

> [!information] Si `snmpwalk` n'est pas installé :
> `apt install snmp snmp-mibs-downloader`

Résultat attendu : des lignes d'informations système du switch (sysDescr, sysName, sysUpTime…)

---

## Étape 3 — Ajouter le switch dans Zabbix

### a. Créer l'hôte

Aller dans `Monitoring > Hosts` puis cliquer sur **Create Host**

| Champ | Valeur |
|---|---|
| Host name | `CBS250-<nom ou IP>` |
| Host groups | `Network devices` *(ou groupe personnalisé)* |
| Interface | Sélectionner **SNMP** |
| IP address | IP du switch CBS 250 |
| Port | `161` |
| SNMP version | `SNMPv2` |
| SNMP community | `{}` |

### b. Définir la macro de communauté

Dans l'onglet **Macros** de l'hôte :

| Macro | Valeur |
|---|---|
| `{}` | `public` *(ou la valeur configurée sur le switch)* |

### c. Associer un template Cisco

Dans l'onglet **Templates**, cliquer sur **Select** :

Rechercher et sélectionner :
`Cisco IOS SNMPv2` *(ou `Cisco Catalyst` selon la version disponible)*

> [!information] Si le template n'est pas présent, l'importer depuis la bibliothèque officielle Zabbix :
> `Data collection > Templates` → **Import**
> Fichier disponible sur : https://www.zabbix.com/integrations/cisco

Cliquer sur **Add** pour finaliser la création de l'hôte.

---

## Étape 4 — Vérifier la supervision

Aller dans `Monitoring > Hosts`

Le switch doit apparaître avec le statut **SNMP** en vert après quelques minutes.

Pour voir les données collectées :
`Monitoring > Latest data` → filtrer par le nom d'hôte du switch

Métriques typiquement remontées par le template Cisco :

| Métrique | Description |
|---|---|
| CPU usage | Charge processeur du switch |
| Memory usage | Utilisation mémoire |
| Interface traffic | Trafic entrant/sortant par port |
| Interface status | État des ports (up/down) |
| System uptime | Durée depuis le dernier redémarrage |

---


## Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| Hôte en rouge / pas de données | Pare-feu bloque UDP 161 | Vérifier les règles réseau entre Zabbix et le switch |
| `snmpwalk` ne répond pas | Communauté incorrecte ou SNMP désactivé | Vérifier la config sur le CBS 250 |
| Template introuvable | Template non importé | Importer depuis la bibliothèque Zabbix |
| Données partielles | Mauvais template (v1 vs v2c) | Vérifier la version SNMP des deux côtés |