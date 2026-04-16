---
base: "[[_Centre de documents.base]]"
Créée par: Maxime COURBOULIN
Catégorie:
  - Déploiement
---
## Prérequis à la mise en place de la procédure 

Posséder un support avec une ISO de Clonezilla

Bonne pratique : avoir un stockage séparé de la partition système

Démarrer sur support :

![CloneZilla](<CloneZilla.png>)

![CloneZilla](<CloneZilla 1.png>)

![CloneZilla](<CloneZilla 2.png>)

## Configuration 

Choix du langage :

![CloneZilla](<CloneZilla 3.png>)

Choix du clavier :

![CloneZilla](<CloneZilla 4.png>)

Changer clavier > 105 touches > Autres > Français AZERTY

![CloneZilla](<CloneZilla 5.png>)

## Choisir l’origine et la destination 

![CloneZilla](<CloneZilla 6.png>)

![CloneZilla](<CloneZilla 7.png>)

![CloneZilla](<CloneZilla 8.png>)

Choisir la destination :

![CloneZilla](<CloneZilla 9.png>)

Vérification :

![CloneZilla](<CloneZilla 10.png>)

Choix du répertoire :

![CloneZilla](<CloneZilla 11.png>)

Style assistant :

![CloneZilla](<CloneZilla 12.png>)

Choix du mode :

![CloneZilla](<CloneZilla 13.png>)

Choix du nom de l’image :

![CloneZilla](<CloneZilla 14.png>)

# Choisir de quel disque faire l’image (Espace pour cocher avec *) :

![CloneZilla](<CloneZilla 15.png>)

Choix du mode de **Savedisk** :

![CloneZilla](<CloneZilla 16.png>)

Vérification et réparation :

![CloneZilla](<CloneZilla 17.png>)

Vérification post installation :

![CloneZilla](<CloneZilla 18.png>)

Chiffrement de l’image :

![CloneZilla](<CloneZilla 19.png>)

Sauvegarde sur Clef USB :

![CloneZilla](<CloneZilla 20.png>)

Tâche après l’installation :

![CloneZilla](<CloneZilla 21.png>)

![CloneZilla](<CloneZilla 22.png>)

Dernière validation :

![CloneZilla](<CloneZilla 23.png>)

Création :

![CloneZilla](<CloneZilla 24.png>)

Il peut arriver que la création ne se lance pas : le message d‘erreur peut être que la partition est en lecture seule. Cela arrive si la partition de destination a été formatée sur Windows ou pas formatée du tout. Dans ce cas suivre les étapes ci-dessous.

## Refus de création, solution possible

_Clonezilla_ refusera pour préparer l’espace de destination, pour cela il est nécessaire de faire la manipulation suivante :

Remonter toutes les fenêtres jusqu’à :

![CloneZilla](<CloneZilla 25.png>)

Puis ajouter les commandes suivantes :

sudo parted /dev/sdb mklabel gpt

sudo parted -a opt /dev/sdb mkpart primary ntfs 0% 100%

sudo mkfs.ntfs -f /dev/sdb1

Explication des commandes :

sudo parted /dev/sdb mklabel gpt

Permet d’exécuter les droits administrateur

Permet de créer, supprimer, redimensionner des partitions et changer la **table de partition**.

Désigne le **disque entier** sur lequel tu veux agir.

Important : **pas une partition** comme /dev/sdb1, mais bien le disque brut.

Tous les contenus du disque **seront effacés** si tu changes la table de partitions.

Make label : type de table de partitions. Définir un nouvelle table écrase l’ancienne.

GUID Partition Table (remplace MBR, Supporte > 2To, jusqu’à 128 partitions sur Linux, robuste : CRC pour vérifier la table)

**⚠️ Ce qu’il faut savoir**

Exécuter sudo parted /dev/sdb mklabel gpt **efface toute table de partition existante** → toutes les données seront perdues.

Après, le disque sera vide, mais tu devras créer **des partitions** avec mkpart ou un outil graphique avant de pouvoir y stocker des fichiers.

sudo parted -a opt /dev/sdb mkpart primary ntfs 0% 100%

-a = **alignement des partitions**

opt = **alignement optimal** pour le disque

Les disques modernes (SSD, Advanced Format HDD) fonctionnent mieux si les partitions sont alignées sur les **secteurs physiques** du disque.

Cela améliore les performances et réduit l’usure sur SSD.

Make partition crée une nouvelle partition sur le disque.

Primaire, correspond au type de partition, sur GPT toutes les partitions sont primaires par défaut.

Type de système de fichier, parted crée la partition sans formater.

0% 100% Début et fin de la partition : **0% = début du disque, 100% = fin du disque**. La partition occupera **tout l’espace disponible** du disque.

sudo mkfs.ntfs -f /dev/sdb1

make filesystem : créer un système de fichiers. Donc mkfs.ntfs formate la partition en **NTFS.**

fast format : formatage rapide, signifie que seule la **table de fichiers** est réinitialisée, et non chaque secteur du disque.

La partition que tu veux formater.