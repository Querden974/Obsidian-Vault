---
tags:
  - proxmox
  - virtualisation
  - vm
  - iso
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Créée par:
  - Gautier RAYEROUX
Date de création: 2026-03-09
---
**Auteur :** Gautier RAYEROUX  |  **Date :** 09/03/2026

---

## 1. Téléverser une image ISO

Les ISO doivent être chargées dans le stockage local de Proxmox avant de créer une VM.

1. Dans l'arborescence de gauche, sélectionner **local (pve)** puis cliquer sur **« ISO Images »**, puis **« Upload »**

![[G_Rayeroux_Procedure_Proxmox_09032026 9.png]]

2. Cliquer sur **« Select File »**, choisir le fichier ISO sur la machine hôte, puis cliquer sur **« Upload »**

![[G_Rayeroux_Procedure_Proxmox_09032026 10.png]]

> [!note]+ Téléchargement depuis une URL
> Le bouton **« Download from URL »** permet de télécharger une ISO directement depuis Internet sans passer par la machine locale.

---

## 2. Créer une machine virtuelle

1. Depuis la vue principale, cliquer sur **« Create VM »** en haut à droite

![[G_Rayeroux_Procedure_Proxmox_09032026 11.png]]

### Onglet General

2. Renseigner :
   - **Node :** `pve` ①
   - **VM ID :** `100` ② (identifiant unique de la VM)
   - **Name :** `Debian13` ③
   
   Cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 12.png]]

### Onglet OS

3. Sélectionner :
   - **Storage :** `local`
   - **ISO image :** `Debian13-file1.iso`
   - **Guest OS Type :** `Linux`
   - **Version :** `6.x - 2.6 Kernel`
   
   Cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 13.png]]

### Onglet System

4. Cocher **« Qemu Agent »** pour activer le guest agent (recommandé pour les VMs Linux/Windows)

![[G_Rayeroux_Procedure_Proxmox_09032026 14.png]]

> [!note]+ QEMU Guest Agent
> Le QEMU Guest Agent permet à Proxmox de communiquer avec la VM (récupération d'IP, shutdown propre, snapshots cohérents). Il doit être installé dans le système invité (`apt install qemu-guest-agent`).

### Onglet Disks

5. Configurer le disque :
   - **Bus/Device :** `SCSI`
   - **Storage :** `local-lvm`
   - **Disk size (GiB) :** `32`
   
   Cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 15.png]]

### Onglet CPU

6. Configurer le processeur :
   - **Sockets :** `1`
   - **Cores :** `1`
   
   Cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 16.png]]

### Onglet Memory

7. Définir la **mémoire RAM** : `2048` MiB (2 Go)

![[G_Rayeroux_Procedure_Proxmox_09032026 17.png]]

### Onglet Network

8. Configurer la carte réseau :
   - **Bridge :** `vmbr0`
   - **Model :** `VirtIO (paravirtualized)`
   
   Cliquer sur **« Next »**

![[G_Rayeroux_Procedure_Proxmox_09032026 18.png]]

### Onglet Confirm

9. Vérifier le récapitulatif de la configuration, puis cliquer sur **« Finish »**

![[G_Rayeroux_Procedure_Proxmox_09032026 19.png]]

> [!tip]+ Démarrer après création
> Cocher **« Start after created »** en bas de la fenêtre pour démarrer la VM automatiquement après sa création.

---

## 3. Démarrer la VM

1. Dans l'arborescence, faire un clic droit sur la VM `100 (Debian13)` et sélectionner **« Start »**

![[G_Rayeroux_Procedure_Proxmox_09032026 20.png]]

2. Cliquer sur **« Console »** dans le menu de la VM pour accéder à l'écran d'installation.

---

## Récapitulatif de la configuration VM (exemple)

| Paramètre | Valeur |
|---|---|
| Nom | Debian13 |
| VM ID | 100 |
| OS | Linux 6.x (Debian 13) |
| Disque | SCSI, 32 GiB, local-lvm |
| CPU | 1 socket × 1 core |
| RAM | 2048 MiB |
| Réseau | VirtIO, bridge vmbr0 |
| QEMU Agent | Activé |
