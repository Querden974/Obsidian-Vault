---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Windows
Heure de la dernière modification: 2026-03-08T18:56:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T18:44:00
---
**Auteur :** Gautier Rayeroux  |  **Date :** 01/11/2025

---

## Préambule

Avant de commencer, assurez-vous que votre machine physique dispose de suffisamment de ressources pour virtualiser une machine.

**Ressources recommandées :** CPU 32 cœurs minimum et 64 Go de RAM.

La virtualisation repose sur un **hyperviseur de type 2**. Plusieurs solutions existent :

- Hyper-V
- VirtualBox
- **VMware Workstation** ← utilisé dans cette procédure

<!-- Column 1 -->
![[imported-image-vShW.png]]


<!-- Column 2 -->
![[imported-image-QURo.png]]


📖 Ressource : [Différence entre hyperviseur type 1 et type 2](https://aws.amazon.com/fr/compare/the-difference-between-type-1-and-type-2-hypervisors/)

---

## 1. Installation de VMware Workstation

### 1.1 Téléchargement de VMware Workstation

1. Rendez-vous sur le site de **Broadcom** et cliquez sur **« Register »**
![[imported-image-GOKT.png]]
2. Créez votre compte et validez votre adresse e-mail
3. Connectez-vous à votre compte
4. Cliquez sur **« My Downloads »** dans le menu de navigation à gauche

![[imported-image-NrL.png]]
5. Cliquez sur **« HERE »** pour accéder aux logiciels
![[imported-image-jbl.png]]
6. Dans la liste, recherchez et cliquez sur **« VMware Workstation Pro »**
![[imported-image-xpX.png]]
![[imported-image-tzoK.png]]
7. Sélectionnez une version **Windows** (ex. : *VMware Workstation Pro 17.0 for Windows*)
8. Cliquez sur la version souhaitée dans la liste
9. Cliquez sur **« Terms and Conditions »**, cochez la case d'acceptation, puis lancez le téléchargement
![[imported-image-JyPw.png]]
10. Si une vérification supplémentaire est demandée, cliquez sur **« Yes »** et remplissez les champs requis

![[imported-image-rPB.png]]
![[imported-image-NDsS.png]]
11. Lancez le téléchargement de l'application

### 1.2 Installation de VMware Workstation

12. Lancez le programme d'installation VMware Workstation
![[imported-image-tKm.png]]
13. Cliquez sur **« Suivant »**
14. Acceptez les **termes d'utilisation**, puis cliquez sur **« Suivant »**
15. Cliquez sur **« Suivant »**
16. Cochez **« Add VMware Workstation console tools into system PATH »**, puis cliquez sur **« Suivant »**
Ne pas oublier de cocher l'ajout au PATH système pour pouvoir utiliser VMware depuis le terminal.
17. Cochez ou non les options facultatives, puis cliquez sur **« Suivant »**
18. Cliquez sur **« Suivant »**
19. Cliquez sur **« Install »**
20. Cliquez sur **« Finish »** pour terminer l'installation

---

## 2. Créer une machine virtuelle

**Exemple :** Dans cette procédure, nous installerons une machine virtuelle **Ubuntu 24.04**.
Téléchargez le fichier `.iso` au préalable sur [ubuntu.com/download](https://ubuntu.com/download)

21. Sur la page **Home** de VMware Workstation, cliquez sur **« Create a New Virtual Machine »**
![[imported-image-iM.png]]
22. Choisissez l'option **« Typical »**, puis cliquez sur **« Next »**
![[imported-image-Qp.png]]
![[imported-image-eQv.png]]
23. Sélectionnez **« Installer disc image file (iso) »**, recherchez l'iso avec **« Browse... »**, puis cliquez sur **« Next »**
24. **Étape optionnelle :** Certains systèmes permettent une personnalisation (nom d'utilisateur, mot de passe…). Remplissez les champs si nécessaire, puis cliquez sur **« Next »**.
![[imported-image-hGgw.png]]
25. Définissez un **nom** et un **emplacement** pour la machine virtuelle, puis cliquez sur **« Next »**
![[imported-image-mLWD.png]]
![[imported-image-rx.png]]
26. Sélectionnez la **taille du disque** et son mode de stockage, puis cliquez sur **« Next »**
27. Sur le récapitulatif, modifiez la configuration hardware si besoin *(modifiable à tout moment)*
![[imported-image-v_L.png]]
28. Cliquez sur **« Finish »** pour terminer la création

Si l'option **« Power on this virtual machine after creation »** est cochée, la machine démarrera automatiquement à la fin de la création.

---

## 3. Modifier une machine virtuelle

**Prérequis :** Assurez-vous que la machine virtuelle est **éteinte** avant de la modifier.

29. Dans la liste de vos machines virtuelles, cliquez sur celle à modifier, puis cliquez sur **« Edit virtual machine settings »**
![[imported-image-CBKN.png]]
30. Dans l'onglet **« Hardware »** : modifiez les ressources allouées (CPU, RAM, disque…)

![[imported-image-qfgs.png]]
![[imported-image-qEPJ.png]]
Vérifiez les ressources disponibles sur votre machine physique avant de configurer celles de la machine virtuelle.
31. Dans l'onglet **« Options »** : configurez les options avancées selon vos besoins

---

## 4. Exporter une machine virtuelle

**Prérequis :** Assurez-vous que la machine virtuelle est **éteinte** avant de l'exporter.

32. Sélectionnez la machine virtuelle à exporter
33. Dans le menu **« File »**, cliquez sur **« Export to OVF... »**
![[imported-image-sWNL.png]]
![[imported-image-Ki.png]]
34. Choisissez le **nom** et l'**emplacement** de destination, puis cliquez sur **« Enregistrer »**
![[imported-image-qLB.png]]
35. Patientez jusqu'à la fin de l'exportation

---

## 5. Importer une machine virtuelle

36. Dans le menu **« File »**, cliquez sur **« Open »**
![[imported-image-om.png]]
![[imported-image-QV_o.png]]
37. Sélectionnez l'emplacement de votre machine virtuelle, puis cliquez sur **« Ouvrir »**
38. Donnez un **nom** à la machine virtuelle et choisissez son **emplacement de sauvegarde**
![[imported-image-jdf.png]]
39. Cliquez sur **« Import »**
40. Une fois importée, configurez la machine selon vos besoins, puis démarrez-la

---

## 6. Utilisation des snapshots

Les snapshots permettent d'effectuer des **sauvegardes de l'état** de la machine virtuelle pour y revenir si besoin.

### 6.1 Créer un snapshot

41. Effectuez un **clic-droit** sur la machine virtuelle
42. Allez dans **« Snapshot »**, puis cliquez sur **« Take Snapshot... »**
![[imported-image-CGu.png]]
![[imported-image-DHzV.png]]
43. Donnez un **nom** et une **description** à la sauvegarde
44. Cliquez sur **« Take Snapshot »**

Vous pouvez retrouver et modifier vos snapshots via le

**« Snapshot Manager »**

.

![[imported-image-lIyL.png]]

![[imported-image-_pwj.png]]

### 6.2 Restaurer un snapshot

45. Ouvrez le **« Snapshot Manager »**
46. Sélectionnez la sauvegarde à restaurer, puis cliquez sur **« Go To »**
![[imported-image-YU.png]]
47. Validez la confirmation de restauration
![[imported-image-qfAz.png]]

La restauration d'un snapshot effacera tous les changements effectués depuis la prise de ce snapshot.