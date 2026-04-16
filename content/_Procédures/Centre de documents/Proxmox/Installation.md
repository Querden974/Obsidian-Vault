---
tags:
  - proxmox
  - virtualisation
  - installation
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Créée par:
  - Gautier RAYEROUX
Date de création: 2026-03-09
---
**Auteur :** Gautier RAYEROUX  |  **Date :** 09/03/2026

---

## Prérequis

### Matériels / Logiciels

- VMware Workstation (si installation en VM)
- Image ISO de **Proxmox VE 7.4** téléchargée depuis [proxmox.com](https://www.proxmox.com/en/downloads)
- Connaissances de base en virtualisation et réseaux

### Configuration utilisée (lab VMware)

| Ressource | Valeur |
|---|---|
| RAM | 4 Go |
| CPU | 2 processeurs, 2 cœurs chacun |
| Disque | 40 Go |
| Réseau | NAT |

---

## 1. Prérequis VMware — Activer la virtualisation imbriquée

Pour faire tourner Proxmox dans VMware Workstation, la virtualisation imbriquée (nested virtualization) doit être activée.

1. Dans les paramètres de la VM VMware, aller dans **Hardware** → **Processors**
2. Cocher **« Virtualize Intel VT-x/EPT or AMD-V/RVI »**

![[G_Rayeroux_Procedure_Proxmox_09032026.png]]

---

## 2. Prérequis VMware — Configurer les dossiers partagés

Les dossiers partagés permettent de transférer des fichiers ISO depuis la machine hôte vers la VM Proxmox.

1. Dans les paramètres de la VM VMware, aller dans **Options** → **Shared Folders**
2. Sélectionner **« Always enabled »**, puis cliquer sur **« Add... »**

![[G_Rayeroux_Procedure_Proxmox_09032026 21.png]]

3. Renseigner le **Host path** (chemin du dossier sur la machine hôte) et un **Name** pour le partage

![[G_Rayeroux_Procedure_Proxmox_09032026 22.png]]

4. Cocher **« Enable this share »** pour activer le partage

![[G_Rayeroux_Procedure_Proxmox_09032026 23.png]]

---

## 3. Installation de Proxmox VE 7.4

Démarrer la VM avec l'ISO Proxmox VE montée.

1. Sélectionner **« Install Proxmox VE »** dans le menu de démarrage

![[G_Rayeroux_Procedure_Proxmox_09032026 1.png]]

2. Lire et accepter l'EULA en cliquant sur **« I agree »**

![[G_Rayeroux_Procedure_Proxmox_09032026 2.png]]

3. Sélectionner le **disque cible** (`/dev/sda`, 40 GiB) puis cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 3.png]]

> [!note]+ Options du disque
> Le bouton **« Options »** permet de choisir le système de fichiers (ext4, xfs, zfs, btrfs). Par défaut : **ext4**.

4. Configurer la **localisation** :
   - **Country :** `France`
   - **Time zone :** `Europe/Paris`
   - **Keyboard Layout :** `French`
   
   Puis cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 4.png]]

5. Définir le **mot de passe root** et l'**adresse e-mail** administrateur, puis cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 5.png]]

> [!warning]+ Sécurité du mot de passe
> Le mot de passe doit comporter au moins 8 caractères avec des lettres, chiffres et symboles. L'email sert aux alertes système (backups, haute disponibilité...).

6. Configurer la **gestion réseau** :
   - **Management Interface :** `ens33`
   - **Hostname (FQDN) :** `pve.tssr.info`
   - **IP Address (CIDR) :** `192.168.92.138 / 24`
   - **Gateway :** `192.168.92.2`
   - **DNS Server :** `192.168.92.2`
   
   Puis cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 6.png]]

7. Vérifier le **résumé de configuration**, puis cliquer sur **« Install »**

![[G_Rayeroux_Procedure_Proxmox_09032026 7.png]]

| Paramètre | Valeur |
|---|---|
| Filesystem | ext4 |
| Disk(s) | /dev/sda |
| Country | France |
| Timezone | Europe/Paris |
| Keymap | fr |
| Hostname | pve |
| IP CIDR | 192.168.92.138/24 |
| Gateway | 192.168.92.2 |
| DNS | 192.168.92.2 |

Patienter pendant l'installation. La machine redémarre automatiquement.

---

## 4. Première connexion à l'interface web

Après le redémarrage, accéder à Proxmox via un navigateur :

```
https://192.168.92.138:8006
```

> [!info]+ Identifiants par défaut
> | Compte | Valeur |
> |---|---|
> | Utilisateur | `root@pam` |
> | Mot de passe | défini à l'étape 5 de l'installation |
