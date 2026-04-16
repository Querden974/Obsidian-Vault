---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Windows
---
Prérequis à la mise en place de la procédure :

Niveau fonctionnel de la forêt `**Windows Server 2008 R2 ou supérieur**`

Droits `**Enterprise Admin**`

L’activation est **irréversible**, on ne peut pas désactiver la corbeille après.

Ouvrir le Centre d’administration Active Directory :

![AD Corbeille](<AD Corbeille.png>)

![AD Corbeille](<AD Corbeille 1.png>)

![AD Corbeille](<AD Corbeille 2.png>)

![AD Corbeille](<AD Corbeille 3.png>)

Redémarrer le Centre d’administration Active Directory.

Test Powershell bonne activation de la corbeille :

![AD Corbeille](<AD Corbeille 4.png>)

`Get-ADOptionalFeature -Filter 'Name -like "Recycle Bin Feature"' | ft Name, EnabledScopes
`
Interroger les **fonctionnalités optionnelles d’Active Directory** (Optional Features).

- **Name** : champ de l’objet AD (nom de la fonctionnalité)
- **like** : opérateur de comparaison partielle
- **"Recycle Bin Feature"** : texte exact qu’on recherche

Le **pipe** sert à prendre la sortie de la commande précédente et à l'envoyer comme entrée de la commande suivante.

Elle indique **dans quelle portée (scope)** la fonctionnalité est activée.