---
tags:
  - proxmox
  - virtualisation
  - hyperviseur
  - index
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Créée par:
  - Gautier RAYEROUX
Date de création: 2026-03-09
---
# Proxmox VE — Centre de Ressources

> [!abstract] Présentation
> **Proxmox Virtual Environment (PVE)** est une plateforme de virtualisation open source basée sur Debian. Elle intègre KVM pour les machines virtuelles et LXC pour les conteneurs, le tout administrable via une interface web. Cette section couvre l'installation, la gestion des VMs, la configuration réseau, les permissions et le stockage iSCSI.

---

## Mise en place

> [!example] Installation
> ### [[Installation]]
> Installation complète de Proxmox VE 7.4 depuis l'ISO, incluant les prérequis VMware pour un déploiement en lab.
>
> - Activation de la virtualisation imbriquée dans VMware (VT-x/AMD-V)
> - Configuration des dossiers partagés VMware
> - Installation de Proxmox VE (EULA, disque, localisation, réseau)

> [!example] Gestion des VM
> ### [[Gestion-des-VM]]
> Upload d'images ISO et création de machines virtuelles dans Proxmox.
>
> - Téléversement d'une ISO dans le stockage local
> - Création d'une VM (General, OS, System, Disks, CPU, Memory, Network)
> - Démarrage de la VM

---

## Configuration

> [!note] Réseau
> ### [[Réseau]]
> Gestion des interfaces réseau, des bridges Linux et des VLANs dans Proxmox.
>
> - Vue des interfaces existantes (ens33, vmbr0)
> - Création d'un Linux Bridge supplémentaire (vmbr1, VLAN-aware)
> - Assignation d'un VLAN Tag à une interface VM
> - Création d'un Linux VLAN (ens33.99)
> - Création d'un bridge sur VLAN existant (vmbr99)

> [!note] Utilisateurs et Permissions
> ### [[Utilisateurs-et-Permissions]]
> Gestion des comptes et des rôles dans Proxmox VE.
>
> - Ajout d'une permission utilisateur (rôle PVEAdmin)
> - Attribution du scope global (`/`)

---

## Stockage

> [!tip] Stockage iSCSI (SAN)
> ### [[Stockage-iSCSI]]
> Configuration d'un stockage iSCSI partagé entre Proxmox et un serveur SAN Debian.
>
> - Activation de SSH root sur le serveur SAN
> - Configuration de targetcli (backstore, target, LUN, portail)
> - Ajout du stockage iSCSI dans Proxmox (Add iSCSI + LVM)
> - Vérification du montage partagé

---

## Comparatif des types de stockage Proxmox

| Type | Cas d'usage | Partage entre nœuds |
|---|---|---|
| `local` | ISO, sauvegardes, CT templates | Non |
| `local-lvm` | Disques VM (raw LVM) | Non |
| `iSCSI` | Stockage partagé via réseau | Oui |
| `LVM sur iSCSI` | Volumes logiques sur SAN | Oui |
| `NFS` | Stockage fichiers partagé | Oui |
| `BTRFS` | Snapshots natifs | Non |

---

> [!info] Accès à l'interface web
> Après installation, Proxmox VE est accessible via :
>
> | Paramètre | Valeur |
> |---|---|
> | URL | `https://<IP>:8006` |
> | Utilisateur par défaut | `root@pam` |
> | Mot de passe | défini à l'installation |
