---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Linux
---


Prérequis : avoir une machine sous Debian (ici Debian 12)

# Installation

Dans le terminal

sudo apt update

sudo apt install borgbackup -y

![BorgBackUp](<BorgBackUp.png>)

## Vérification de version

![BorgBackUp](<BorgBackUp 1.png>)

## Création utilisateur dédié Borg (sécurité)

![BorgBackUp](<BorgBackUp 2.png>)

# IP statique

Sudo nano /etc/network/interfaces

![BorgBackUp](<BorgBackUp 3.png>)