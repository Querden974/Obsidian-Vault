---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Linux
---

# Passer la machine debian en IP statique

```sh
sudo nano /etc/network/interfaces
```

![SAN Debian12 2](<SAN Debian12 2.png>)

# Vérifier la présence de iscsi

```sh
dpkg -l | grep targetcli
dpkg -l | grep open-iscsi
```

Si aucun retour, ils ne sont pas installés.

# Installer iscsi

```sh
apt update
apt install -y targetcli-fb open-iscsi
```

Gestionnaire de paquet debian, outil standard pour installer des logiciels.

Action à faire.

Oui automatique pour installer sans poser de questions.

Serveur iSCSI (créer des targets iSCSI, expose les disques virtuels, gère les accès réseau, gère l’authentification)

Option côté serveur qui permet e se connecter à un SAN iSCSI, tester le SAN depuis la VM

ATTENTION le open-iscsi, et utile sur le serveur SAN dans le cadre d’un lab. Dans les conditions réelles de production seul les clients auront open-iscsi.

## Vérifier le status du service

```sh
sudo systemctl status rtslib-fb-targetctl
```

![SAN Debian12 2](<SAN Debian12 2 1.png>)

## Identifier le SAN

```sh
lsblk
```

![SAN Debian12 2](<SAN Debian12 2 2.png>)

Dans la configuration actuelle nous ne pouvons monter le SAN car nous ne devons pas le monter dans le sda : cela pourrait entraîner l’écrasement du système d’exploitation.

Nous devons ajouter un nouveau disque :

![SAN Debian12 2](<SAN Debian12 2 3.png>)

# Configuration SAN

```sh
sudo targetcli
```

![SAN Debian12 2](<SAN Debian12 2 4.png>)

Message d’alerte normale, il indique que le fichier des préférences n’existe pas, logique au premier lancement, le fichier sera créé lors de la sauvegarde de la configuration avec _/saveconfig_.

## Vérification fonctionnement de shell

```targetcli
/> ls
```

![SAN Debian12 2](<SAN Debian12 2 5.png>)

## Création du backstore (disque physique pour le SAN)

```targetcli
/backstores/block create name=disk1 dev=/dev/sdb
```

Emplacement où sont stockés les disques exposés.

Nom interne que tu donnes au backstore (arbitraire)

Destination

![SAN Debian12 2](<SAN Debian12 2 6.png>)

Après modification :

![SAN Debian12 2](<SAN Debian12 2 7.png>)

Création point d’accès réseau

```targetcli
/iscsi create iqn.2026-02.SAN.local:diskSAN
```

- `iqn.2026-02.SAN.local :diskSAN` identifiant unique, convention iSCSI
- année/mois, convention iSCSI
- domaine
- nom du target

![SAN Debian12 2](<SAN Debian12 2 8.png>)

# Création LUN pour lier disque dur et target

```targetcli
/iscsi/iqn.2026-02.san.local:disksan/tpg1/luns create /backstores/block/diskSAN
```

![SAN Debian12 2](<SAN Debian12 2 9.png>)

A ce stade la configuration ressemble à ça :

![SAN Debian12 2](<SAN Debian12 2 10.png>)

# Sauvegarde de la configuration

![SAN Debian12 2](<SAN Debian12 2 11.png>)

# Installation client iSCSI sur machine cliente

```sh
sudo apt update
sudo apt install -y open-iscsi
```

# Découvrir le target sur le serveur SAN

Ici `192.168.199.100` est l’adresse IP du serveur san. Sur la machine cliente :

```sh
sudo iscsiadm -m discovery -t sendtargets -p 192.168.199.100
```

outil en ligne de commande pour gérer le client iSCSI

mode de fonctionnement `discovery` pour découvrir les targets iSCSI sur un serveur.

Type de découverte dédiée aux targets.

Type de destination, ici une adresse IP.

Adresse IP du serveur SAN.

![SAN Debian12 2](<SAN Debian12 2 12.png>)

## Se connecter au SAN

```sh
sudo iscsiadm -m node -l
```

![SAN Debian12 2](<SAN Debian12 2 13.png>)

![SAN Debian12 2](<SAN Debian12 2 14.png>)

Attention pour se connecter le client doit être explicitement enregistré comme autorisé dans les ACL.

## Problème courant

Le client ne voie pas le disksan (sdb) :

![SAN Debian12 2](<SAN Debian12 2 15.png>)

Test :

![SAN Debian12 2](<SAN Debian12 2 16.png>)  

### Dans le serveur SAN

```sh
targetcli
```

![SAN Debian12 2](<SAN Debian12 2 17.png>)

```targetcli
/iscsi/iqn.2026-02.san.local:disksan/tpg1
```

Solution possible depuis le serveur SAN

#### Supprimer le portail en 0.0.0.0 (conflit vmware)

Supprimer le portail en 0.0.0.0 :

![SAN Debian12 2](<SAN Debian12 2 18.png>)

Recréer le portail avec l’adresse de la VM SAN :

![SAN Debian12 2](<SAN Debian12 2 19.png>)

Saveconfig

![SAN Debian12 2](<SAN Debian12 2 20.png>)

#### Ajout de l’utilisateur dans les ACL

```targetcli
/iscsi/iqn.2026-02.san.local:disksan/tpg1/acls create iqn.1993-08.org.debian:01:9349b1adda55
```

(Pour retrouver l’IQN du client : _cat /etc/iscsi/initiatorname.iscsi_ )

![SAN Debian12 2](<SAN Debian12 2 21.png>)

Vérifier : ls

![SAN Debian12 2](<SAN Debian12 2 22.png>)

Saveconfig

![SAN Debian12 2](<SAN Debian12 2 23.png>)

Résultat :

![SAN Debian12 2](<SAN Debian12 2 24.png>)

### Dans le client  
Vérifier la présence du disque SAN

lsblk

![SAN Debian12 2](<SAN Debian12 2 25.png>)

#### Formater le disque dur

```sh
sudo mkfs.ext4 /dev/sdb
```

![SAN Debian12 2](<SAN Debian12 2 26.png>)

#### Créer un point de montage

```sh
sudo mkdir -p /mnt/iscsi
```

`make directory` créer un dossier

Créer tout les dossiers parents nécessaires, s’ils existent déjà, empêche de renvoyer une erreur

Chemin complet du dossier

![SAN Debian12 2](<SAN Debian12 2 27.png>)

#### Monter le disque

```sh
sudo mount /dev/sdb1 /mnt/iscsi
```

![SAN Debian12 2](<SAN Debian12 2 28.png>)

#### Vérification

```sh
df -h
```

![SAN Debian12 2](<SAN Debian12 2 29.png>)

# Mise en garde

Dans un projet de SAN avec un seul espace de stockage réparti sur plusieurs disques, nous devons créer un espace logique côté serveur SAN avant d’exposer le LUN.

Pour qu’un dossier placé dans le SAN par le client 1 soit visible par le client 2, il faut utiliser un système de fichiers `cluster-aware`.