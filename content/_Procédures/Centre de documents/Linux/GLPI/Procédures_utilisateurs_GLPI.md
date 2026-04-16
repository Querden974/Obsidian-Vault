---
base: "[[_Centre de documents.base]]"
Créée par:
  - God IBRAHIM
  - Alexander BINDER
  - Louis MALONGA NZEMBE
Catégorie:
  - Linux
Date de création:
---

---

## 1. Activer l'inventaire dans GLPI

Connectez-vous à l'interface d'administration GLPI via votre nom de domaine ou l'adresse IP de votre serveur.

> [!note]
> Depuis GLPI 11, l'inventaire est intégré nativement — il n'est plus nécessaire d'installer le plugin FusionInventory. L'inventaire est cependant **désactivé par défaut**.

Pour l'activer :

1. Cliquer sur **« Administration »** dans le menu latéral
2. Cliquer sur **« Inventaire »**
3. Cocher l'option **« Activer l'inventaire »**
4. Cliquer sur **« Sauvegarder »** en bas de page

![[Procédures_utilisateurs_GLPI.png]]

Pour importer les éléments du parc, voir la section [Agents GLPI](#2-agents-glpi).

---

## 2. Agents GLPI

### 2.1 Installation sur Debian / Linux

1. Ouvrir le terminal et mettre à jour le système :
```plain text
sudo apt update
```
2. Installer les paquets nécessaires :
```plain text
sudo apt install wget perl -y
```
3. Récupérer le lien de la dernière version de l'agent sur la page GitHub : [GLPI Agent Releases](https://github.com/glpi-project/glpi-agent/releases/tag/1.7)

Faire un clic droit sur le fichier `.pl` et copier le lien.

![[Procédures_utilisateurs_GLPI.jpeg]]

4. Télécharger l'agent (remplacer l'URL par le lien copié) :
```plain text
wget <lien-copié>
```
![[Procédures_utilisateurs_GLPI 1.jpeg]]
![[Procédures_utilisateurs_GLPI 2.jpeg]]

#### Installer l'agent

5. Lancer l'installation :
```plain text
perl glpi-agent-1.7-linux-installer.pl --type=all --install
```
6. Saisir l'URL du serveur GLPI lorsqu'elle est demandée (ex. : `http://192.168.159.129/`)

![[Procédures_utilisateurs_GLPI 3.jpeg]]

7. Appuyer sur **Entrée** pour conserver la configuration par défaut

![[Procédures_utilisateurs_GLPI 4.jpeg]]

L'agent est maintenant installé.

![[Procédures_utilisateurs_GLPI 5.jpeg]]

#### Activer et démarrer le service

8. Activer le service au démarrage :
```plain text
systemctl enable glpi-agent
```
![[Procédures_utilisateurs_GLPI 8.png]]
9. Démarrer le service :
```plain text
systemctl start glpi-agent
```
10. Vérifier le statut :
```plain text
systemctl status glpi-agent
```
![[Procédures_utilisateurs_GLPI 6.jpeg]]
11. Déclencher un inventaire manuellement pour tester :
```plain text
sudo glpi-agent --force
```
![[Procédures_utilisateurs_GLPI 7.jpeg]]

---

### 2.2 Installation sur Windows

1. Télécharger le fichier `GLPI-Agent-*-x64.msi` depuis la page GitHub : [GLPI Agent Releases](https://github.com/glpi-project/glpi-agent/releases/tag/1.7)

![[Procédures_utilisateurs_GLPI 8.jpeg]]

2. Lancer l'installation et renseigner l'URL du serveur GLPI pour faire remonter l'inventaire

![[Procédures_utilisateurs_GLPI 9.jpeg]]

3. Après l'installation, redémarrer le service via :
```plain text
services.msc
```
Faire un clic droit sur **GLPI-Agent** → **Redémarrer**

![[Procédures_utilisateurs_GLPI 10.jpeg]]

#### Test de l'agent

4. Se connecter à l'interface locale de l'agent dans un navigateur :
```plain text
http://127.0.0.1:62354
```
![[Procédures_utilisateurs_GLPI 11.jpeg]]

5. Cliquer sur **« Force an inventory »** pour vérifier que l'agent est opérationnel et pointe bien vers le serveur GLPI

![[Procédures_utilisateurs_GLPI 12.jpeg]]

---

## 3. Parc informatique

Après avoir importé les éléments du parc grâce aux agents, il est possible d'administrer l'inventaire du matériel.

### 3.1 Tableau de bord

Accéder au tableau de bord général selon le profil technicien :

![[Procédures_utilisateurs_GLPI 1.png]]

Sélectionner ensuite le type d'élément à administrer dans le menu déroulant (ex. : **Ordinateurs**) :

![[Procédures_utilisateurs_GLPI 2.png]]

### 3.2 Fiche d'un élément

GLPI permet de relier différents éléments matériels, logiciels et d'administration :

1. Les éléments liés à la machine sont listés à droite
2. Il est possible d'ajouter des paramètres précis via des gabarits
3. La machine peut être attribuée à un utilisateur et à un groupe, avec la possibilité d'ajouter des commentaires
4. Un numéro d'inventaire peut lui être attribué
5. Un QR code est généré automatiquement pour faciliter la gestion administrative
6. Pensez à sauvegarder les modifications — la machine peut être mise à la corbeille puis supprimée

![[Procédures_utilisateurs_GLPI 3.png]]

Sous la fiche de la machine, le gestionnaire de l'agent indique :

1. La machine à laquelle il est attaché
2. L'adresse publique de contact
3. Son statut
4. Les informations sur l'agent (avec possibilité de redemander un inventaire)
5. La date de la dernière mise à jour de l'inventaire
6. La possibilité de télécharger l'inventaire en fichier `.json`

![[Procédures_utilisateurs_GLPI 4.png]]

### 3.3 Gabarits

Il est possible de créer des gabarits pour standardiser les éléments :

1. Sélectionner **« Gabarits »**
2. Choisir le type de gabarit à droite
3. Paramétrer le gabarit
4. Nommer le gabarit
5. Sauvegarder

![[Procédures_utilisateurs_GLPI 5.png]]

### 3.4 Créer un élément manuellement

1. Sélectionner le type d'élément
2. Cliquer sur **« Ajouter »** pour ouvrir l'interface de création
3. Nommer l'élément
4. Paramétrer ses caractéristiques
5. Ajouter les paramètres d'identification
6. Cliquer sur **« Ajouter »**

![[Procédures_utilisateurs_GLPI 6.png]]

---

## 4. Installation de l'agent monitor (Windows)

L'agent monitor permet de déclencher des inventaires et de créer des tickets directement depuis la barre des tâches Windows.

1. Télécharger le fichier `.exe` depuis la page GitHub : [GLPI Agent Monitor Releases](https://github.com/glpi-project/glpi-agentmonitor/releases)

![[Procédures_utilisateurs_GLPI 13.jpeg]]

2. Lancer l'installation

![[Procédures_utilisateurs_GLPI 14.jpeg]]

3. Cliquer sur **« Paramètres »**, renseigner l'URL du serveur GLPI, puis cliquer sur **« Enregistrer »**

![[Procédures_utilisateurs_GLPI 15.jpeg]]

4. Cliquer sur **« Forcer l'inventaire »** pour tester

![[Procédures_utilisateurs_GLPI 16.jpeg]]

> [!note]
> Il est aussi possible de créer un ticket directement en cliquant sur **« Nouveau ticket »** — cela redirige vers le tableau de bord du serveur GLPI.

---

## 5. Base de connaissance

Dans GLPI, il est possible d'enrichir la base de connaissance via **Outils** → **Base de connaissance** → **Ajouter**.

![[Procédures_utilisateurs_GLPI 9.png]]

### 5.1 Ajouter un article

1. Choisir la catégorie dans l'arborescence
2. Cocher **« FAQ »** si l'article doit y apparaître
3. Définir une date d'accès
4. Saisir le titre
5. Rédiger le contenu
6. Joindre un fichier si nécessaire
7. Préciser le groupe ou profil ayant accès à l'information

![[Procédures_utilisateurs_GLPI 10.png]]

### 5.2 Ajouter une catégorie

Cliquer sur le **+** dans l'arborescence pour créer une catégorie :

1. Définir le nom de la catégorie
2. Ajouter un commentaire
3. Définir sa position dans l'arborescence
4. Cliquer sur **« Ajouter »**

![[Procédures_utilisateurs_GLPI 11.png]]
![[Procédures_utilisateurs_GLPI 12.png]]

### 5.3 Consulter un article

Pour accéder à une information, effectuer une recherche par mot-clé ou :

1. Sélectionner **« Base de connaissance »**
2. Dans **« Parcourir »**, sélectionner la catégorie
3. Sélectionner l'article souhaité

![[Procédures_utilisateurs_GLPI 13.png]]

Le contenu de l'article s'affiche :

![[Procédures_utilisateurs_GLPI 14.png]]
