---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Linux
---
Prérequis à la mise en place de la procédure :

Debian 12

Iptables installé (sudo apt install iptables)

Netfilter-persistent installé (sudo apt install netfilter-persistent iptables-persistent)

# Mise à jour système

Sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt autoclean &&

Sudo apt clean

# Installation

Sudo apt install -y glusterfs-server && sudo systemctl status glusterd

![GlusterFS](<GlusterFS.png>)

Installation EPEL (Extra Packages for Entreprise Linux)

_sudo systemctl enable --now glusterd_

![GlusterFS](<GlusterFS 1.png>)

_sudo iptables -A INPUT -p tcp --dport 24007 -j ACCEPT_

_sudo iptables -A INPUT -p tcp --dport 24008 -j ACCEPT_

_sudo iptables -A INPUT -p tcp --dport 49152:49251 -j ACCEPT_

_sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT_

_sudo netfilter-persistent save_

![GlusterFS](<GlusterFS 2.png>)

_sudo systemctl restart glusterd_

_sudo systemctl enable glusterd_

![GlusterFS](<GlusterFS 3.png>)

Activer le service

Configurer le pare-feu pour autoriser le trafic

Redémarrer

## Nommage du serveur

_sudo hostnamectl set-hostname_ **_nom_du_serveur_**

_sudo reboot_

ech![GlusterFS](<GlusterFS 4.png>)

# Configuration des serveurs

_echo -e_ **_“IP_du_serveur1 nom_du_serveur1_**_\n_**_IP_du_serveur2 nom_du_serveur2_**_\n_**_IP_du_serveur3 nom_du_serveur3_**_“ | sudo tee -a /etc/hosts_

![GlusterFS](<GlusterFS 5.png>)

# Configuration hôtes

![GlusterFS](<GlusterFS 6.png>)

Ajouter les pairs des servers pour qu’ils se reconnaissent

Sudo gluster peer probe IP_serveur1

Sudo gluster peer probe IP_serveur2

Sudo gluster peer probe IP_serveur3

Créer les répertoires qui sera utilisé pour la réplication

Sudo mkdir -p /Applicatif

# Création volume GlusterFS

Choix du serveur 1 comme serveur maître

Sudo gluster volume create volum_applicatif replica 3 arbiter 1 transport tcp IP_serveur1:/Applicatif IP_serveur2:/Applicatif IP_serveur3:/Applicatif force

![GlusterFS](<GlusterFS 7.png>)

Sudo gluster volume start volume_applicatif

Créer le volume 3 répliqué et 1 arbitre

Démarre le volume

# Accorder les droits et monter le volume

_sudo mkdir -p /mnt/montage_applicatif_

_echo “ ip_serveur1:/volume_applicatif /mnt/montage_applicatif glusterfs defaults,_netdev 0 0“ | sudo tee -a /etc/fstab_

_sudo systemctl daemon-reload_

_sudo mount -a_

Créer un dossier, tous les dossiers parents si nécessaires, vers ce point de montage

Imprime le texte « ... », ajoute (-a) à la fin du fichier /etc/fstab

Recharger la configuration systemd

Monter tous les fichiers de fstab

# Changer les droits sur les serveurs

sudo groupadd gluster_apache

sudo usermod -aG gluster_apache apache

sudo usermod -aG gluster_apache gluster

sudo chmod -R 755/Applicatif

sudo chown -R :gluster_apache /Applicatif

sudo chmod -R 755 /mnt/montage_applicatif

sudo chmod -R : /mnt/montage_applicatif

Créer un groupe gluster_apache, ajouter les utilisateurs apache et gluster à ces groupes, ajouter les permissions sur les répertoires « /Applicatif » et « /mnt/montage_applicatif ».

## Vérifier le montage 

Df -h

# Script de vérification et maintenance cluster GlusterFS

Sudo nano /usr/local/bin/glusterfs-check.sh