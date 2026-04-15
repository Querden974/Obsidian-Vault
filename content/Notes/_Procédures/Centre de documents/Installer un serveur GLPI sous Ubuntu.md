---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-03-08T19:11:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T18:53:00
---
**Auteur :** Gautier  |  **Date :** 31/10/2025  |  **Version GLPI :** 11.0.2

---

## Prérequis

Une machine virtuelle ou hôte sous **Ubuntu Server** doit être disponible et installée.

Passez sous le profil **super utilisateur** pour l'ensemble de la procédure.

### Passer en super utilisateur

1. Entrer la commande suivante :
```plain text
sudo su --
```
![[imported-image-GhRJ.png]]
2. Entrer le mot de passe, puis appuyer sur **Entrée**

---

## 1. Installer les prérequis

3. Mettre à jour le serveur :
```plain text
apt update && apt upgrade
```
![[imported-image-VIFL.png]]
Patienter jusqu'à la fin de la mise à jour.
4. Installer les dépendances (Apache, PHP, MariaDB et extensions) :
```plain text
apt install -y apache2 php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,redis,bz2} libapache2-mod-php php-soap php-cas
apt install -y mariadb-server
```

---

## 2. Configurer la base de données

### 2.1 Installation sécurisée de MariaDB

5. Lancer la configuration sécurisée :
```plain text
mysql_secure_installation
```
![[imported-image-xLUC.png]]

**Recommandations lors de la configuration :**

- Changer le mot de passe par défaut
- Supprimer les utilisateurs anonymes
- Désactiver l'accès root à distance
- Supprimer la base de données test
- Réinitialiser les privilèges

### 2.2 Créer la base de données et l'utilisateur GLPI

6. Se connecter à MariaDB et exécuter les commandes suivantes :
```plain text
mysql -uroot -pmysql

CREATE DATABASE glpi;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY '***yourstrongpassword***';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
GRANT SELECT ON `mysql`.`time_zone_name` TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
```
![[imported-image-mlr.png]]
7. Quitter MariaDB :
```plain text
exit
```
![[imported-image-LJ.png]]

---

## 3. Préparer les fichiers d'installation GLPI

8. Se déplacer dans le dossier web, télécharger et décompresser GLPI :
```plain text
cd /var/www/html
wget https://github.com/glpi-project/glpi/releases/download/11.0.2/glpi-11.0.2.tgz
tar -xvzf glpi-11.0.2.tgz
```
![[imported-image-QXae.png]]
9. Supprimer l'archive :
```plain text
rm glpi-11.0.2.tgz
```
![[imported-image-TZ.png]]

### 3.1 Structure des dossiers (FHS)

- `/etc/glpi` — fichiers de configuration
- `/var/www/html/glpi` — code source (lecture seule, servi par Apache)
- `/var/lib/glpi` — fichiers variables (sessions, documents, cache, plugins…)
- `/var/log/glpi` — fichiers journaux

### 3.2 Configuration du fichier « downstream.php »

10. Créer le fichier :
```plain text
nano /var/www/html/glpi/inc/downstream.php
```
![[imported-image-MwoY.png]]
11. Y insérer le contenu suivant :
```plain text
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```
12. Sauvegarder : **Ctrl+X** → **Y** → **Entrée**
13. Déplacer les fichiers vers leurs nouvelles destinations :
```plain text
mv /var/www/html/glpi/config /etc/glpi
mv /var/www/html/glpi/files /var/lib/glpi
mv /var/lib/glpi/_log /var/log/glpi
```
![[imported-image-ONYK.png]]

### 3.3 Configuration du fichier « local_define.php »

14. Créer le fichier :
```plain text
nano /etc/glpi/local_define.php
```
![[imported-image-COyy.png]]
15. Y insérer les définitions de variables :
```plain text
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi');
define('GLPI_DOC_DIR', GLPI_VAR_DIR);
define('GLPI_CACHE_DIR', GLPI_VAR_DIR . '/_cache');
define('GLPI_CRON_DIR', GLPI_VAR_DIR . '/_cron');
define('GLPI_GRAPH_DIR', GLPI_VAR_DIR . '/_graphs');
define('GLPI_LOCAL_I18N_DIR', GLPI_VAR_DIR . '/_locales');
define('GLPI_LOCK_DIR', GLPI_VAR_DIR . '/_lock');
define('GLPI_PICTURE_DIR', GLPI_VAR_DIR . '/_pictures');
define('GLPI_PLUGIN_DOC_DIR', GLPI_VAR_DIR . '/_plugins');
define('GLPI_RSS_DIR', GLPI_VAR_DIR . '/_rss');
define('GLPI_SESSION_DIR', GLPI_VAR_DIR . '/_sessions');
define('GLPI_TMP_DIR', GLPI_VAR_DIR . '/_tmp');
define('GLPI_UPLOAD_DIR', GLPI_VAR_DIR . '/_uploads');
define('GLPI_INVENTORY_DIR', GLPI_VAR_DIR . '/_inventories');
define('GLPI_THEMES_DIR', GLPI_VAR_DIR . '/_themes');
define('GLPI_LOG_DIR', '/var/log/glpi');
```
16. Sauvegarder : **Ctrl+X** → **Y** → **Entrée**

---

## 4. Permissions des dossiers et fichiers

```plain text
chown root:root /var/www/html/glpi/ -R
chown www-data:www-data /etc/glpi -R
chown www-data:www-data /var/lib/glpi -R
chown www-data:www-data /var/log/glpi -R
chown www-data:www-data /var/www/html/glpi/marketplace -Rf
find /var/www/html/glpi/ -type f -exec chmod 0644 {} \;
find /var/www/html/glpi/ -type d -exec chmod 0755 {} \;
find /etc/glpi -type f -exec chmod 0644 {} \;
find /etc/glpi -type d -exec chmod 0755 {} \;
find /var/lib/glpi -type f -exec chmod 0644 {} \;
find /var/lib/glpi -type d -exec chmod 0755 {} \;
find /var/log/glpi -type f -exec chmod 0644 {} \;
find /var/log/glpi -type d -exec chmod 0755 {} \;
```

---

## 5. Configurer le serveur Web (VirtualHost Apache)

17. Créer le fichier de configuration :
```plain text
nano /etc/apache2/sites-available/glpi.conf
```
![[imported-image-ci.png]]
18. Y coller la configuration suivante :
```plain text
<VirtualHost *:80>
    ServerName glpi.localhost
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Require all granted
        RewriteEngine On
        RewriteCond %{HTTP:Authorization} ^(.+)$
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
```
19. Sauvegarder : **Ctrl+X** → **Y** → **Entrée**
20. Activer le VirtualHost :
```plain text
a2dissite 000-default.conf   # Désactive le site par défaut
a2enmod rewrite              # Active le module de réécriture
a2ensite glpi.conf           # Active la config VirtualHost
systemctl restart apache2    # Redémarre Apache
```

### 5.1 Configuration PHP.ini

Paramètres recommandés pour un bon fonctionnement de GLPI.

21. Ouvrir le fichier :
```plain text
nano /etc/php/8.3/apache2/php.ini
```
22. Modifier les paramètres suivants :
    - `upload_max_filesize = 20M`
    - `post_max_size = 20M`
    - `max_execution_time = 60`
    - `max_input_vars = 5000`
    - `memory_limit = 256M`
    - `session.cookie_httponly = On`
    - `date.timezone = Europe/Paris`

---

## 6. Démarrer l'installation web

23. Récupérer l'adresse IP du serveur :
```plain text
ip a
```
![[imported-image-QuCu.png]]
24. Accéder à l'adresse IP dans un navigateur (ex. : `192.168.254.139`)
25. Cliquer sur **« Aller à la page d'installation »**
![[imported-image-zVAq.png]]
26. Sélectionner la langue, puis cliquer sur **« OK »**
![[imported-image-gSd.png]]
27. Lire la licence, puis cliquer sur **« Continuer »**
![[imported-image-HNPT.png]]
28. Cliquer sur **« Installer »**
![[imported-image-fcm.png]]
29. Vérifier que tous les tests de compatibilité sont concluants, puis cliquer sur **« Continuer »**
![[imported-image-XU.png]]
30. Renseigner le serveur SQL et les identifiants de la base de données, puis cliquer sur **« Continuer »**
![[imported-image-dBiM.png]]
31. Sélectionner la base de données `glpi`, cliquer sur **« Continuer »** et patienter
![[imported-image-Dmju.png]]
32. Cocher ou non l'envoi de statistiques, puis cliquer sur **« Continuer »**
![[imported-image-PXFX.png]]
33. Cliquer sur **« Continuer »**
![[imported-image-RuPN.png]]
34. Cliquer sur **« Utiliser GLPI »**
![[imported-image-oPR.png]]

---

## 7. Connexion à GLPI

35. Utiliser les identifiants par défaut : **glpi / glpi**
![[imported-image-GORw.png]]
36. Une fois connecté, configurer le serveur GLPI selon vos besoins
![[imported-image-Qrmu.png]]

Changez les mots de passe par défaut dès la première connexion.