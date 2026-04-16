---
Créée par: Gautier RAYEROUX
Catégorie:
  - Linux
Date de création: 2026-04-15T00:00:00
---

# Récapitulatif — Capture et Déploiement d'image FOG

> [!abstract] Prérequis
> Le serveur FOG est installé et opérationnel, le DHCP est configuré avec les options PXE (`next-server` + fichier de boot).
> Les postes clients doivent pouvoir **booter sur le réseau (PXE)** depuis leur BIOS/UEFI.

---

# Capture d'une image

## Étape 1 — Préparer le master

- Installer Windows et effectuer toutes les personnalisations (logiciels, paramètres)
- Installer les mises à jour Windows Update
- Nettoyer les fichiers système : `Clic droit C: > Propriétés > Nettoyage de disque > Nettoyer les fichiers système`
- Désactiver le **démarrage rapide** (sinon le disque reste verrouillé et FOG ne peut pas lire les données)

> [!warning] Important
> Ne pas installer de pilotes matériels spécifiques si le master est destiné à être déployé sur des machines de configuration différente.

---

## Étape 2 — Créer la définition de l'image dans FOG

Aller dans `Images > Create New Image` et renseigner :

| Champ | Valeur recommandée |
|---|---|
| **Image Name** | Nom explicite (ex: `Win11-Master-Compta`) |
| **Image Type** | `Multiple Partition Image - Single Disk` |
| **OS** | Sélectionner le système correspondant |
| **Storage Group** | Laisser le groupe par défaut |

Cliquer sur **Add** pour enregistrer.

---

## Étape 3 — Enregistrer le poste master dans FOG

1. Booter le poste master **sur le réseau (PXE)**
2. Dans le menu iPXE FOG, choisir **`Quick Registration and Inventory`**
3. Le poste apparaît dans `Hosts > List All Hosts` avec un nom généré automatiquement
4. Renommer l'hôte si nécessaire et lui **associer l'image** créée à l'étape 2 :
   `Hosts > <nom du poste> > Image > sélectionner l'image > Update`

---

## Étape 4 — Lancer la tâche de capture

Aller dans `Hosts > <nom du poste> > Basic Tasks > Capture`

> [!tip] Lancer immédiatement
> Cocher **`Schedule Instantly`** puis cliquer sur **`Task Now`**.

Redémarrer le poste — il bootera automatiquement en PXE et commencera la capture.

> [!note] Suivi
> L'avancement est visible dans `Task Management > Active Tasks`.

---

## Étape 5 — Vérifier la capture

Une fois terminée, l'image apparaît dans `Images > <nom de l'image>` avec une taille renseignée.
Le poste redémarre automatiquement sur son disque local.

---

---

# Déploiement d'une image

## Étape 1 — Enregistrer le poste cible dans FOG

1. Booter le poste cible **sur le réseau (PXE)**
2. Choisir **`Quick Registration and Inventory`** dans le menu iPXE
3. Le poste apparaît dans `Hosts > List All Hosts`
4. Lui **associer l'image** à déployer :
   `Hosts > <nom du poste> > Image > sélectionner l'image > Update`

---

## Étape 2 — Lancer la tâche de déploiement

Aller dans `Hosts > <nom du poste> > Basic Tasks > Deploy`

Cocher **`Schedule Instantly`** puis cliquer sur **`Task Now`**.

Redémarrer le poste — il bootera en PXE et recevra l'image.

---

## Étape 3 — Déploiement sur plusieurs machines (groupe)

Pour déployer simultanément sur un parc de postes :

1. Aller dans `Groups > <nom du groupe>` (ou créer le groupe via `Groups > Create New Group`)
2. Ajouter les hôtes cibles au groupe
3. Associer l'image au groupe : `Groups > <groupe> > Image > sélectionner > Update`
4. Lancer : `Groups > <groupe> > Basic Tasks > Deploy > Task Now`

> [!warning] Attention
> Tous les postes du groupe doivent être **allumés et en attente de boot PXE** au moment du lancement de la tâche.

---

## Étape 4 — Vérification post-déploiement

- Le poste redémarre automatiquement sur le disque local après le déploiement
- Vérifier que Windows démarre correctement et que les personnalisations sont bien présentes
- Si nécessaire, effectuer une **sysprep** en amont de la capture pour que Windows génère un nouveau SID à chaque déploiement

> [!tip] Sysprep (optionnel mais recommandé en production)
> Exécuter sur le master **avant la capture**, depuis `C:\Windows\System32\Sysprep\` :
> ```
> sysprep.exe /oobe /generalize /shutdown
> ```
> Le poste s'éteint — lancer la capture FOG immédiatement après.

---

## Récapitulatif visuel

```
CAPTURE
───────
Master prêt → Créer image dans FOG → Enregistrer le poste (PXE)
→ Associer l'image à l'hôte → Lancer "Capture" → Vérifier

DÉPLOIEMENT
───────────
Enregistrer le(s) poste(s) cible(s) (PXE) → Associer l'image
→ Lancer "Deploy" (hôte ou groupe) → Vérifier au redémarrage
```
