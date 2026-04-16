---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Linux
---

Prérequis à la mise en place de la procédure :

Table adressage IP

Connexion internet

# Gluster

## IP static

Nano /etc/network/interfaces

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa.png>)

## Définition du nom de la machine

sudo hostnamectl set-hostname gluster

nano /etc/hosts

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 1.png>)

## Installation

sudo apt update && sudo apt upgrade –y

sudo apt install glusterfs-server –y

sudo systemctl enable glusterd

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 2.png>)

sudo systemctl start glusterd

### Vérification

sudo systemctl status glusterd

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 3.png>)

## Création du dossier brick

sudo mkdir -p /data/gluster/brick1

### Gestion des droits

## Nouveau disque

lsblk

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 4.png>)

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 5.png>)

fdsik /dev/sdb

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 6.png>)

### Vérification

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 7.png>)

### Formatage xfs

(adapté pour le projet)

mkfs.xfs -i size=512 /dev/sdb1

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 8.png>)

## Créer un point de montage

mkdir -p /data/brick1

## Monter le disque

mount /dev/sdb1 /data/brick1

## Vérification

df -h /data/brick1

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 9.png>)

## Rendre le montage permanent

### Récupère l’UUID

blkid /dev/sdb1

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 10.png>)

### Ajoute dans /etc/fstab

UUID=xxxxx /data/brick1 xfs defaults 0 0

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 11.png>)

### Test

mount -a

ne renvoie rien en cas de bon fonctionnement

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 12.png>)

## Vérification finale GlusterFS

df -h /data/brick1

Réponse visée :

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 13.png>)

## Création de mon volume

gluster volume create monvolume SAN:/data/brick1 force

gluster volume start monvolume

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 14.png>)

### Vérifie

gluster volume info

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 15.png>)

gluster volume status

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 16.png>)

## Installation de FUSE

Apt install fuse -y

Modprobe fuse

Lsmod | grep fuse

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 17.png>)

# Machine cliente

## IP Statique

Nano /etc/network/interfaces

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 18.png>)

## Mettre à jour /etc/hosts avec les interlocuteurs

Nano /etc/hosts

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 19.png>)

## Installer le client gluster

sudo apt install glusterfs-client –y

## Créer un point de montage

sudo mkdir -p /mnt/gluster

## Monter le volume Gluster

sudo mount -t glusterfs 192.168.199.100:/gv0 /mnt/gv0

ls /mnt/gv0 # vérifier que c'est accessible

## Tester le montage du volume Gluster

ls /mnt/gluster # vérifier si le volume est accessible

touch /mnt/gluster/test.txt # tester l’écriture

ls -l /mnt/gluster # vérifier que le fichier a été créé

# BorgBackup

## IP statique

nano /etc/network/interfaces

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 20.png>)

## Mettre à jour /etc/hosts avec les interlocuteurs

Nano /etc/hosts

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 21.png>)

## Installation

sudo apt update

sudo apt install borgbackup –y

## Installation client gluster

sudo apt install glusterfs-client –y

### Test ping borg > gluster

Ping gluster

Ping gluster.gluster.san.local

![Procédure projet SAN BORG CLIENT afpa](<Procédure projet SAN BORG CLIENT afpa 22.png>)

## Créer un point de montage

sudo mkdir -p /mnt/gluster

## Monter le volume Gluster

sudo mount -t glusterfs 192.168.75.100:/gv0 /mnt/gluster

ls /mnt/gluster # vérifier que c'est accessible