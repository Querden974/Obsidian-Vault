---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Windows
Heure de la dernière modification: 2026-03-08T18:57:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T18:54:00
---
**Auteur :** Gautier Rayeroux  |  **Date :** 31/10/2025

---

## Prérequis

Avant de commencer, rassembler les informations suivantes :

- Marque de l'imprimante
- Modèle de l'imprimante
- Adresse IP

**Exemple :** Marque : Xerox  |  Modèle : VersaLink B415  |  Adresse IP : 172.12.200.2

Assurez-vous que l'imprimante est **sous tension** avant de commencer.

---

## 1. Vérifier la visibilité de l'imprimante

1. Ouvrir l'**Invite de commande**
2. Effectuer un ping vers l'adresse IP de l'imprimante :
```plain text
ping 172.12.200.2
```

Si le ping est concluant, passez à la suite.

---

## 2. Télécharger le driver

3. Sur votre moteur de recherche, chercher le pilote de l'imprimante
Ex : *Xerox VersaLink B415 driver*
![[imported-image-NgKb.png]]
4. Sélectionner la plateforme du système d'exploitation et appliquer les filtres
![[imported-image-IpN.png]]
5. Trouver le programme d'installation, **accepter les conditions de vente**, puis cliquer sur **« Télécharger »**
![[imported-image-iu.png]]

Prenez connaissance des conditions de vente avant de télécharger.

6. Lancer le programme d'installation une fois le téléchargement terminé
![[imported-image-GA.png]]

---

## 3. Installer le pilote

7. Accepter le contrat de licence
![[imported-image-aS.png]]

Prenez connaissance du contrat de licence avant d'accepter.

8. Trouver l'imprimante dans la liste, cliquer sur **« Installer »** et laisser le processus se terminer

---

## 4. Configurer l'imprimante

### 4.1 Nommer l'imprimante

9. Dans la barre de recherche Windows, chercher **« Imprimantes »**
10. Cliquer sur l'imprimante à configurer, puis sélectionner **« Gérer »**
11. Cliquer sur **« Configurer l'imprimante »**, puis renommer selon la politique de nommage
Ex : `IMP_Bat10_Xerox_Rdc_10`

### 4.2 Tester l'imprimante

12. Sur la page de gestion, cliquer sur **« Imprimer une page test »**

Si la page s'imprime correctement, l'installation est terminée ✅