---
Créée par: Gautier RAYEROUX
Catégorie:
  - Windows
  - Déploiement
Date de création: 2026-04-15T00:00:00
Source: https://neptunet.fr/capture-master-wds/
base: "[[_Centre de documents.base]]"
---

# Capture d'une image master avec WDS

> [!abstract] Objectif
> Déployer un Windows préconfigué sur des postes clients **sans support physique**, en capturant un master personnalisé sous forme de fichier `.wim` directement sur le serveur **WDS (Windows Deployment Services)**.

---

## Prérequis

| Élément | Détail |
|---|---|
| Serveur | Windows Server 2019, **IP fixe**, disque secondaire pour WDS |
| Client master | Windows 10 à personnaliser |
| Réseau | DHCP actif, les deux machines sur le même réseau |
| ISO Windows 10 | Nécessaire pour extraire le fichier `boot.wim` |

> [!tip] Bonne pratique
> Créer un **compte utilisateur dédié** aux déploiements plutôt que d'utiliser le compte Administrateur.

---

## 1. Préparer le serveur

- Renommer le serveur
- Attribuer une **adresse IP fixe**
- Ajouter un **disque de stockage secondaire** pour les fichiers WDS

---

## 2. Installer et configurer WDS

Dans le **Gestionnaire de serveur**, ajouter le rôle **Windows Deployment Services**, puis configurer avec les paramètres suivants :

| Paramètre | Valeur |
|---|---|
| Mode | **Standalone** (sans Active Directory) |
| Dossier RemoteInstall | Sur le disque secondaire (ex : `E:\RemoteInstall`) |
| Approbation des ordinateurs | Accepter tous les ordinateurs sans approbation |
| Boot PXE | Continuer automatiquement |

---

## 3. Créer l'image de capture

### a. Importer le fichier boot.wim

Monter l'ISO Windows 10 et importer le fichier source :
```
[Lecteur ISO]:\Sources\boot.wim
```
dans WDS via `Images de démarrage > Ajouter une image de démarrage`.

### b. Générer l'image de capture

Faire un clic droit sur l'image de démarrage importée → **Créer une image de capture**.

Enregistrer le fichier `.wim` généré dans :
```
E:\RemoteInstall\Boot\x64\Images\[nom_capture].wim
```

### c. Configurer les images actives

Dans WDS :
- **Désactiver** l'image de démarrage (Setup)
- **Garder uniquement** l'image de capture active

> [!note]
> Créer également un **groupe d'images d'installation** vide pour accueillir le `.wim` capturé plus tard.

---

## 4. Préparer le master Windows

### a. Installer Windows 10 normalement

Vérifier la connectivité réseau avec le serveur :
```cmd
ping <IP_serveur>
```

Installer les **mises à jour Windows** et les **logiciels souhaités**.

### b. Lancer Sysprep en mode audit

Ouvrir une invite de commandes en tant qu'administrateur :
```cmd
C:\Windows\System32\Sysprep\Sysprep.exe /audit /reboot
```

Le poste redémarre en **mode audit** (compte Administrateur intégré, sans OOBE).
Effectuer ici les personnalisations supplémentaires : logiciels, modifications du registre, etc.

### c. Généraliser le master avec Sysprep

Une fois les personnalisations terminées :
```cmd
C:\Windows\System32\Sysprep\Sysprep.exe /generalize /oobe /shutdown
```

Le poste s'éteint automatiquement — **ne pas le rallumer manuellement**.

> [!warning] Important
> Aucune mise à jour ne doit être en cours d'exécution pendant le Sysprep.
> En cas d'erreur, consulter les logs ici :
> ```
> C:\Windows\System32\Sysprep\Panther\
> ```

---

## 5. Capturer l'image via PXE

### a. Booter le master sur le réseau

Démarrer le poste master en **boot réseau** (touche `F12` au démarrage ou réglage dans le BIOS/UEFI).

Sélectionner l'option **LAN / PXE boot** — le menu WDS se charge.

### b. Lancer l'assistant de capture WDS

Dans le menu WDS, sélectionner l'**image de capture** précédemment créée.

Suivre l'assistant :
VM-WinServ2019.virtual-ad.local

| Étape | Action |
|---|---|
| Sélectionner le volume | Choisir la partition contenant Windows (généralement `C:`) |
| Nommer l'image | Donner un nom explicite (ex : `Win10-Master-Compta`) |
| Connexion au serveur | Saisir `nom_serveur\nom_utilisateur` |
| Destination | Partage réseau `\\nom_serveur\reminst` |
| Nom du fichier `.wim` | Inclure l'extension `.wim` (ex : `Win10-Master-Compta.wim`) |

> [!note] Alternative
> Il est possible d'enregistrer le `.wim` sur une **clé USB**, un disque externe ou un partage réseau tiers avant de l'importer manuellement dans WDS.

Lancer la capture — **l'opération prend plusieurs minutes**.

---

## 6. Préparer WDS pour le déploiement

Une fois la capture terminée et le `.wim` disponible sur le serveur :

- **Réactiver** l'image de démarrage (Setup)
- **Désactiver** l'image de capture
- Vérifier que le `.wim` capturé est bien visible dans le groupe d'images d'installation WDS

---

## 7. Tester le déploiement

1. Créer une machine virtuelle vierge sur le même réseau
2. Booter sur le réseau (PXE)
3. S'authentifier avec `nom_serveur\nom_utilisateur`
4. Sélectionner l'image d'installation personnalisée
5. Laisser l'installation se dérouler jusqu'à l'OOBE
6. Vérifier que les personnalisations du master sont bien présentes

---

## Récapitulatif des chemins importants

| Élément | Chemin |
|---|---|
| Source boot.wim | `[Lecteur ISO]:\Sources\boot.wim` |
| Stockage image de capture | `E:\RemoteInstall\Boot\x64\Images\` |
| Partage réseau WDS | `\\nom_serveur\reminst` |
| Logs Sysprep | `C:\Windows\System32\Sysprep\Panther\` |

---

> [!tip] Pour aller plus loin
> Utiliser un fichier **unattend.xml** pour automatiser complètement le déploiement (choix de la langue, création du compte, activation…) et supprimer les interactions manuelles lors de l'installation.
