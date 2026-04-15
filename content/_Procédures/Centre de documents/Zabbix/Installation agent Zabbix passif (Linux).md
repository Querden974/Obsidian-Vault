---
---

# Installation d'un agent Zabbix passif sur Linux

> [!note]+ Mode passif
> En mode **passif**, c'est le **serveur Zabbix** qui initie la connexion vers l'agent pour collecter les métriques (polling). L'agent écoute sur le port **10050/TCP** et répond aux requêtes du serveur.

## Prérequis

- OS : Debian 12/13 ou Ubuntu 22.04/24.04
- Accès root ou sudo
- Connectivité réseau avec le serveur Zabbix
- Port **10050/TCP** ouvert en entrée sur la machine cible

---

## 1. Ajouter le dépôt Zabbix

> [!tip]+ Choisir la bonne version
> Adapter l'URL selon la version de Zabbix du serveur et la distribution. Vérifier sur https://www.zabbix.com/download

### Debian 12

```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
apt update
```

### Ubuntu 22.04

```bash
wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_7.2+ubuntu22.04_all.deb
apt update
```

---

## 2. Installer l'agent Zabbix

```bash
apt install -y zabbix-agent
```

> [!note]+ Agent vs Agent 2
> `zabbix-agent` (v1) suffit pour la majorité des cas. `zabbix-agent2` offre plus de plugins mais nécessite Go runtime. Cette procédure utilise `zabbix-agent`.

---

## 3. Configurer l'agent

Éditer le fichier de configuration :

```bash
nano /etc/zabbix/zabbix_agentd.conf
```

### Paramètres essentiels

| Paramètre | Valeur | Description |
|---|---|---|
| `Server` | IP du serveur Zabbix | Adresse autorisée à interroger l'agent (mode passif) |
| `ServerActive` | *(laisser vide ou commenter)* | Non utilisé en mode purement passif |
| `Hostname` | Nom de l'hôte (identique à Zabbix UI) | Doit correspondre exactement au nom défini dans l'interface |
| `ListenPort` | `10050` | Port d'écoute de l'agent (défaut) |
| `ListenIP` | `0.0.0.0` | Interface d'écoute (toutes par défaut) |

### Exemple de configuration minimale

```ini
# Serveur Zabbix autorisé à interroger l'agent
Server=192.168.1.10

# Désactiver les checks actifs (mode passif uniquement)
# ServerActive=

# Nom de l'hôte tel que déclaré dans Zabbix
Hostname=srv-web-01

# Port d'écoute
ListenPort=10050
```

> [!warning]+ Attention au Hostname
> Le champ `Hostname` doit être **identique** au nom de l'hôte configuré dans l'interface Zabbix, sinon les données ne seront pas associées.

---

## 4. Ouvrir le port pare-feu

### Avec UFW (Ubuntu/Debian)

```bash
ufw allow 10050/tcp
ufw reload
```

### Avec firewalld (RHEL/CentOS)

```bash
firewall-cmd --permanent --add-port=10050/tcp
firewall-cmd --reload
```

### Avec iptables

```bash
iptables -A INPUT -p tcp --dport 10050 -s 192.168.1.10 -j ACCEPT
```

---

## 5. Démarrer et activer le service

```bash
systemctl enable zabbix-agent
systemctl start zabbix-agent
systemctl status zabbix-agent
```

La sortie doit afficher `active (running)`.

---

## 6. Vérifier le bon fonctionnement

### Tester la connectivité depuis le serveur Zabbix

Sur le **serveur Zabbix**, utiliser `zabbix_get` pour interroger l'agent :

```bash
# Installation de zabbix-get si absent
apt install zabbix-get

# Test de récupération d'une métrique
zabbix_get -s 192.168.1.50 -p 10050 -k agent.version
```

> Remplacer `192.168.1.50` par l'IP de la machine cible.

Une réponse du type `7.2.x` confirme que l'agent répond correctement.

### Vérifier les logs de l'agent

```bash
tail -f /var/log/zabbix/zabbix_agentd.log
```

---

## 7. Ajouter l'hôte dans l'interface Zabbix

1. Se connecter à l'interface web Zabbix
2. Aller dans **Monitoring → Hosts → Create host**
3. Renseigner les champs :

| Champ | Valeur |
|---|---|
| **Host name** | Identique au `Hostname` du fichier de conf |
| **Templates** | `Linux by Zabbix agent` (ou équivalent) |
| **Host groups** | Groupe approprié |
| **Interfaces** | Ajouter une interface **Agent** avec l'IP de la machine |
| **Port** | `10050` |

4. Cliquer sur **Add**

> [!note]+ Vérification dans l'UI
> Après quelques minutes, l'icône de disponibilité de l'hôte doit passer au **vert** dans la liste des hôtes.

---

## Dépannage

| Symptôme | Cause possible | Solution |
|---|---|---|
| Icône rouge dans Zabbix | Agent inaccessible | Vérifier pare-feu et service |
| `Connection refused` | Service arrêté | `systemctl start zabbix-agent` |
| `Cannot connect to` | IP serveur incorrecte dans `Server=` | Corriger `zabbix_agentd.conf` |
| Données manquantes | `Hostname` ne correspond pas | Harmoniser le nom hôte |
| `zabbix_get` timeout | Port 10050 bloqué | Vérifier les règles firewall |
