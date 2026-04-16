# Création de comptes utilisateurs et attribution des droits
### Ajouter AccessControl
 
Aller dans `Fog configuration > Fog Settings > Plugins system`:
Cocher la case `Pluginsys Enable`, puis `Update`.
![[Mise en place serveur FOG-1776166147031.jpeg]]

Une nouvelle icone va apparaitre pour la gestion des plugins.
![[Mise en place serveur FOG-1776166313810.jpeg]]

Dans la liste des plugins, chercher `accesscontrol`
Aller dans `Plugins > Install Plugins`,  cliquer sur le plugin `accesscontrol`, puis cliquer sur `Install`.
![[Mise en place serveur FOG-1776166524254.jpeg]]

### Créer un utilisateur

Aller dans le menu `User` puis cliquer sur `Create New User`.
![[Mise en place serveur FOG-1776166663165.jpeg]]

Renseigner les différents champs
![[Mise en place serveur FOG-1776166714696.jpeg]]
> [!information]+ User API Enabled
> Permet l’autorisation de l’API REST qui est une interface qui permet à des programmes externes de communiquer avec FOG. Cela permet notamment l’utilisation de scripts pour lancer des tâches de déploiement à distance ou créer des hôtes automatiquement.

Une fois créée, l'utilisateur doit apparaitre dans la page `All Users`.
![[Mise en place serveur FOG-1776166818046.jpeg]]

### Créer un rôle

Cliquer sur l'icone de `Access Control` dans la barre d'outils, puis cliquer sur `Add new role`.
![[Mise en place serveur FOG-1776166977004.jpeg]]

Remplir le nom du rôle et lui donner une description.

Une fois le rôle créée, cliquer sur `Add new rule`.
#### **Règle pour Autoriser l'enregistrement de machines** 
Type : Host 
Parent : List 
Nœud parent : laisser vide ou tapez root 
Valeur de la règle : create,list,read 
#### **Règle pour Autoriser le déploiement d’images** 
Type : Task 
Parent : List/Create 
Nœud parent : laisser vide ou tapez root 
Valeur de la règle : create,list 
#### **Règle pour protéger les images de la modification** 
Type : Image 
Parent : List 
Nœud parent : laisser vide ou tapez root 
Valeur de la règle : list,read

Pour associer les règles aux rôles, aller dans `List all roles`, sélectionner le rôle concerné, cocher les règles créées précédemment puis cliquer sur ajouter.
![[Mise en place serveur FOG-1776167525604.jpeg]]

Les règles appliqué au rôle devrait apparaître.
![[Mise en place serveur FOG-1776167569418.jpeg]]

### Assigner un rôle à un utilisateur

Aller dans `Users > List all Users`, sélectionner l'utilisateur à appliquer le rôle, puis dans le champ `User Access Control` sélectionner le rôle à assigner et cliquer sur `Update`.
![[Mise en place serveur FOG-1776167704988.jpeg]]
