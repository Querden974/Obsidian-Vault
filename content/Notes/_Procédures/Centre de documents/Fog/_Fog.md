---
tags:
  - fog
  - déploiement
  - imaging
  - index
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
  - Déploiement
Heure de la dernière modification: 2026-04-15T13:28:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T13:29:00
---

# FOG Project — Centre de Ressources

> [!abstract] Présentation
> **FOG Project** est une solution open-source de clonage et de déploiement d'images système via le réseau. Elle permet de capturer, stocker et déployer des images disque sur des postes clients à partir d'un boot PXE, sans support physique.
> 
> Interface web : **http://\<IP\>/fog/management** | Boot PXE : **TFTP** | Stockage images : **/images**

---

> [!warning] Prérequis réseau
> Le déploiement FOG repose sur le **boot PXE**, qui nécessite un serveur **DHCP** et un **DNS** correctement configurés sur le réseau.
> 
> - Mise en place d'un serveur DHCP → [[Mise en place serveur DHCP]]
> - Mise en place d'un serveur DNS → [[Mise en place d'un serveur DNS (Bind9)]]

---

## Installation & Configuration

> [!example] Serveur FOG
> ### [[Mise en place serveur FOG]]
> Installation complète du serveur FOG sur Debian, configuration du boot PXE et paramétrage de l'interface web.
> 
> - Installation depuis les sources GitHub
> - Choix du mode (Normal Server / Storage Node)
> - Intégration DHCP existant (options 066/067)
> - Initialisation de la base de données
> - Paramétrage du Menu Timeout iPXE
> - Création d'un groupe d'images

---

## Images & Déploiement

--- start-multi-column: FogImages
```column-settings
number of columns: 2
Column Size: [50%, 50%]
```

> [!info] Création d'une image Windows
> ### [[Création d'une image Windows]]
> Préparer un master Windows avant la capture.
> 
> - Personnalisation du master (logiciels, mises à jour)
> - Nettoyage des fichiers système
> - Désactivation du démarrage rapide

--- end-column ---

> [!info] Récapitulatif — Capture et Déploiement
> ### [[Récapitulatif - Capture et Déploiement]]
> Toutes les étapes de A à Z pour capturer et déployer une image.
>
> - Créer la définition d'image dans FOG
> - Enregistrer les postes via boot PXE
> - Lancer les tâches Capture / Deploy
> - Déploiement sur groupe de machines
> - Sysprep avant capture (production)

--- end-multi-column

---

## Gestion des accès

> [!info] Gestion d'utilisateurs
> ### [[Gestion d'utilisateurs]]
> Créer des comptes utilisateurs, définir des rôles et appliquer des restrictions d'accès via le plugin AccessControl.
> 
> - Activation du système de plugins
> - Installation du plugin `accesscontrol`
> - Création d'utilisateurs et de rôles
> - Définition de règles (Host, Task, Image)
> - Association rôle ↔ utilisateur

---

## Rappels essentiels

| Élément | Valeur par défaut |
|---|---|
| Identifiants par défaut | `fog` / `password` |
| Interface web | `http://<IP>/fog/management` |
| Stockage des images | `/images` |
| Boot BIOS | `undionly.kpxe` |
| Boot UEFI 64-bit | `ipxe.efi` |
| Option DHCP next-server (066) | IP du serveur FOG |
| Option DHCP filename (067) | Fichier de boot PXE |

---

> [!tip] Flux de mise en place rapide
> 1. Vérifier la présence d'un **serveur DHCP** configuré avec les options PXE → [[Mise en place serveur DHCP]]
> 2. Vérifier la présence d'un **serveur DNS** → [[Mise en place d'un serveur DNS (Bind9)]]
> 3. Installer et configurer le **serveur FOG** → [[Mise en place serveur FOG]]
> 4. Préparer le **master Windows** → [[Création d'une image Windows]]
> 5. Suivre le **récapitulatif capture/déploiement** → [[Récapitulatif - Capture et Déploiement]]
> 6. Gérer les **accès utilisateurs** selon les besoins → [[Gestion d'utilisateurs]]

