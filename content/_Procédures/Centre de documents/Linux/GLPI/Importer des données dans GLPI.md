---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-03-08T18:58:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T18:53:00
---
**Auteur :** Gautier Rayeroux  |  **Date :** 24/11/2025

---

## Prérequis

Vous devez au préalable avoir installé un serveur GLPI.

Passez sous le profil **super utilisateur** pour avoir tous les privilèges.

### Passer en super utilisateur

1. Entrer la commande suivante :
```plain text
sudo su --
```
![[imported-image-um.png]]
2. Entrer le mot de passe du compte super utilisateur, puis appuyer sur **Entrée**

---

## 1. Télécharger l'archive « datainjection »

Entrer les commandes suivantes pour télécharger et extraire le plugin :

```plain text
cd /var/www/html/glpi/plugins
wget https://github.com/pluginsGLPI/datainjection/releases/download/2.15.1/glpi-datainjection-2.15.1.tar.bz2
tar -xvf glpi-datainjection-2.15.1.tar.bz2
```

---

## 2. Installer le plugin

3. Sur l'interface GLPI, aller dans **« Configuration »** → **« Plugins »**
4. Sur la ligne **« Data Injection »**, activer le plugin

---

## 3. Créer un modèle d'injection

5. Dans **« Outils »**, cliquer sur **« Data Injection »**, puis sur l'icône **« Modèle »**
6. Cliquer sur **« Ajouter »**
7. Remplir les champs, choisir la catégorie de données à injecter, puis cliquer sur **« Ajouter »**
8. Sélectionner le fichier CSV à injecter pour préparer le modèle
9. Effectuer les correspondances entre les en-têtes CSV et les champs de la catégorie, puis cliquer sur **« Sauvegarder »**
10. Aller dans l'onglet **« Validation »**, puis cliquer sur **« Valider le modèle »**

---

## 4. Importer le fichier

11. Aller dans **« Outils »** → **« Data Injection »**
12. Sélectionner le modèle d'injection
13. Sélectionner le fichier CSV contenant les données
14. Cliquer sur **« Procéder à l'injection »**