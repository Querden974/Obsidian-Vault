---
tags:
  - proxmox
  - stockage
  - iscsi
  - san
  - lvm
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Créée par:
  - Gautier RAYEROUX
Date de création: 2026-03-09
---
**Auteur :** Gautier RAYEROUX  |  **Date :** 09/03/2026

---

## Présentation

L'objectif est de connecter Proxmox à un **serveur SAN** Debian exposant un disque via le protocole **iSCSI**. Proxmox pourra ensuite créer des volumes LVM sur ce stockage partagé, utilisables par les VMs.

### Architecture

```
[Serveur SAN Debian]              [Proxmox VE]
  /dev/sdb (20 GiB)   ──iSCSI──▶  iscsi (storage)
  targetcli / LIO                  iscsi-lvm (LVM sur iSCSI)
```

---

## 1. Prérequis — Activer SSH root sur le serveur SAN

Pour accéder au serveur SAN depuis Proxmox via SSH, l'authentification root doit être autorisée.

1. Sur le serveur SAN Debian, éditer `/etc/ssh/sshd_config` et s'assurer que la ligne suivante est présente et décommentée :

```
PermitRootLogin yes
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 34.png]]

2. Redémarrer le service SSH :

```bash
systemctl restart ssh
```

> [!warning]+ Sécurité
> `PermitRootLogin yes` est acceptable en lab. En production, préférer `PermitRootLogin prohibit-password` avec une clé SSH.

---

## 2. Configuration iSCSI côté SAN — targetcli

`targetcli` est l'outil de configuration du framework **LIO** (Linux I/O Target) pour exposer des disques via iSCSI.

### Installation

```bash
apt install targetcli-fb rtslib-fb
```

### Lancer targetcli

```bash
targetcli
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 35.png]]

### Créer un backstore block

Un **backstore block** associe un périphérique bloc physique à un objet de stockage utilisable par iSCSI.

```
/backstores/block create disk01 /dev/sdb
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 36.png]]

Vérifier la création :

```
ls /backstores/block
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 37.png]]

### Créer un target iSCSI

Un **target** est le point d'accès exposé aux initiateurs (clients iSCSI). La commande génère automatiquement un IQN unique.

```
/iscsi create
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 38.png]]

> [!info]+ IQN (iSCSI Qualified Name)
> Format : `iqn.<année>-<mois>.<domaine-inversé>:<identifiant>`
> Exemple : `iqn.2003-01.org.linux-iscsi.san.x8664:sn.35b7f014fbc5`

Vérifier la structure du target (TPG, ACLs, LUNs, Portals) :

```
ls /iscsi
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 39.png]]

### Créer un LUN

Un **LUN** (Logical Unit Number) associe le backstore au target pour le rendre accessible.

```
/iscsi/<IQN>/tpg1/luns create /backstores/block/disk01
```

Exemple :

```
/iscsi/iqn.2003-01.org.linux-iscsi.san.x8664:sn.35b7f014fbc5/tpg1/luns create /backstores/block/disk01
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 40.png]]

### Vérifier la configuration complète

```
ls
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 41.png]]

### Sauvegarder et quitter

```
saveconfig
exit
```

### Vérifier le service au démarrage

```bash
systemctl enable rtslib-fb-targetctl
systemctl status rtslib-fb-targetctl
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 42.png]]

---

## 3. Ajouter le stockage iSCSI dans Proxmox

### Ajouter le storage iSCSI

1. Dans Proxmox, aller dans **Datacenter** → **Storage** → **« Add »** → **« iSCSI »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 43.png]]

2. Renseigner :
   - **ID ①** : nom du stockage (ex. `iscsi`)
   - **Portal ②** : adresse IP du serveur SAN (ex. `192.168.92.x`)
   - Une fois le portail renseigné, le **Target** se découvre automatiquement

### Ajouter un volume LVM sur l'iSCSI

Pour créer des volumes logiques (disques VM) sur le stockage iSCSI, ajouter un LVM par-dessus.

3. Dans **Datacenter** → **Storage** → **« Add »** → **« LVM »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 44.png]]

4. Renseigner :
   - **ID ①** : `iscsi-lvm`
   - **Base storage ②** : `iscsi (iSCSI)` (le storage iSCSI créé précédemment)
   - **Base volume ③** : `CH 00 ID 0 LUN 0` (le LUN exposé par le SAN)
   - **Volume group ④** : `vg-iscsi`
   - **Content ⑤** : `Disk image, Container`
   
   Cliquer sur **« Add »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 45.png]]

> [!note]+ Shared
> L'option **Shared** permet à plusieurs nœuds Proxmox d'accéder au même stockage LVM. Cocher si le stockage est partagé dans un cluster.

---

## 4. Vérification — Stockage partagé entre VMs

Pour confirmer que le stockage iSCSI est bien monté et accessible depuis plusieurs VMs :

1. Sur une VM connectée au stockage partagé, vérifier la configuration réseau :

```bash
ip addr
```

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 46.png]]

La VM affiche deux adresses sur `ens33` :
- `172.12.50.5/16` (adresse DHCP)
- `172.12.50.100/24` (adresse IP partagée — VIP)

2. Accéder à l'IP partagée depuis un navigateur → le **SERVEUR MASTER** répond :

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 47.png]]

3. Lors d'une bascule (failover), la même IP répond depuis le **SERVEUR BACKUP** :

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 48.png]]

> [!success]+ Résultat
> Le stockage iSCSI partagé permet aux deux VMs (MASTER et BACKUP) d'accéder aux mêmes données. La VIP `172.12.50.100` bascule automatiquement entre les serveurs en cas de panne.

---

## Récapitulatif des commandes targetcli

| Commande | Description |
|---|---|
| `targetcli` | Lancer le shell targetcli |
| `/backstores/block create <nom> <device>` | Créer un backstore block |
| `/iscsi create` | Créer un target iSCSI (IQN auto) |
| `/iscsi/<IQN>/tpg1/luns create /backstores/block/<nom>` | Créer un LUN |
| `ls` | Afficher toute la configuration |
| `saveconfig` | Sauvegarder la configuration |
| `exit` | Quitter targetcli |
