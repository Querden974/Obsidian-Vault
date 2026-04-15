# Recomendation
|Installation size|Monitored metrics**1**|CPU/vCPU cores|Memory  <br>(GiB)|Database|Amazon EC2**2**|
|---|---|---|---|---|---|
|Small|1 000|2|8|MySQL Server,  <br>Percona Server,  <br>MariaDB Server,  <br>PostgreSQL|m6i.large/m6g.large|
|Medium|10 000|4|16|MySQL Server,  <br>Percona Server,  <br>MariaDB Server,  <br>PostgreSQL|m6i.xlarge/m6g.xlarge|
|Large|100 000|16|64|MySQL Server,  <br>Percona Server,  <br>MariaDB Server,  <br>PostgreSQL|m6i.4xlarge/m6g.4xlarge|
|Very large|1 000 000|32|96|MySQL Server,  <br>Percona Server,  <br>MariaDB Server,  <br>PostgreSQL|m6i.8xlarge/m6g.8xlarge|

|Platform|Server|Agent|Agent 2|Comments|
|---|---|---|---|---|
|Linux|x|x|x||
|Windows|-|x|x|Zabbix agent is supported on all desktop and server versions since Windows XP (64-bit)/Server 2003.  <br>  <br>Zabbix agent 2 is supported on all desktop and server versions since Windows 10 (32-bit)/Server 2016, as it is compiled only with a [supported Go version](https://www.zabbix.com/documentation/current/en/manual/installation/requirements#agent-2) to prevent critical security vulnerabilities. Since Go 1.21, the [minimum required Windows versions](https://go.dev/wiki/MinimumRequirements#windowswindows) are raised, making Windows 10/Server 2016 the minimum version for Zabbix agent 2.|
|macOS|x|x|-||
|IBM AIX|x|x|-|Zabbix agent does not work on AIX platforms below versions 6.1 TL07 / 7.1 TL01.|
|FreeBSD|x|x|-||
|OpenBSD|x|x|-||
|Solaris|x|x|-||
|NetBSD|x|x|-||
|HP-UX|x|x|-||

# Prérequis
- OS: Debian 13
- RAM: 2Gb
- Stockage: 30Go

# Installation
## 1. Choisir la plateforme
![[_Zabbix-1775631792355.jpeg]]
## 2. Installation et configuration de Zabbix
### a. Installer les dépots Zabbix
```
wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb   
dpkg -i zabbix-release_latest_7.4+debian13_all.deb   
apt update
```
### b. Installer Zabbix Serveur, Frontend, agent2
`apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent2 mariadb-server`
### c. Installer les plugins Zabbix agent2
`apt install zabbix-agent2-plugin-mongodb zabbix-agent2-plugin-mssql zabbix-agent2-plugin-postgresql`
### d. Créer la base de données initiale
Vérifier qu'un service de base de données soit activé et en cours de fonctionnement.
Exécuter les commandes suivantes sur l'hôte de la base de données:
```
# mysql -uroot -p   
password   
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;   
mysql> create user zabbix@localhost identified by 'password';   
mysql> grant all privileges on zabbix.* to zabbix@localhost;   
mysql> set global log_bin_trust_function_creators = 1;   
mysql> quit;
```
Sur le serveur hôte, importer les schémas et les données initiaux, il va être nécessaire d'entrer le mot de passé créée pour mariaDB.
`zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix`

Désactiver l'option `log_bin_trust_function_creators` après l'importation du schéma de la base de données.
```
# mysql -uroot -p   
password   
mysql> set global log_bin_trust_function_creators = 0;   
mysql> quit;
```
### e. Configurer la base de donnée du serveur Zabbix
Modifier le fichier `/etc/sabbix/zabbix_server.conf`
```
DBPassword=Afpa123*   # Décommenter la ligne !!!
```
> [!warning]  Il est important de décommenter la ligne avant de sauvegarder le fichier de configuration.
### f. Modifier le fichier de configuration nginx de Zabbix
Editer le fichier `/etc/zabbix/nginx`
```
server {
        listen          8080;         # Décommenter la ligne
        server_name     example.com;  # Décommenter la ligne

        root    /usr/share/zabbix/ui;
```
### g. Démarrer le serveur et les agents Zabbix
Démarrer les services et activez les au démarrage de l'hôte.
```
systemctl restart zabbix-server zabbix-agent2 nginx php8.4-fpm   
systemctl enable zabbix-server zabbix-agent2 nginx php8.4-fpm
```

### h. Ouvrir l'interface web Zabbix
Récupérer l'addresse IP du serveur hôte
> [!information] Utiliser l'addresse de la machine hôte avec le port configuré dans le fichier nginx de Zabbix
> `http://192.168.92.146:8080`


## 3. Configuration interface WEB
![[_Zabbix-1775635237875.jpeg]]

Vérifier que tout les prérequis ont tous le statut "<font color="#92d050">OK</font>"
![[_Zabbix-1775635342127.jpeg|750]]

Renseigner le mot de passe de la base de données.
![[_Zabbix-1775635482568.jpeg]]

Renseigner un nom de serveur et définir le fuseau horraire.
![[_Zabbix-1775635580343.jpeg]]

Vérifier la configuration pour l'installation de Zabbix
![[_Zabbix-1775635603359.jpeg]]

Une fois l'installation terminé, cliquer sur "Finish"
![[_Zabbix-1775635664085.jpeg]]

> [!information] Pour se connecter la première fois, utiliser les identifiants par défaut: `Admin / zabbix`

## 4. Création d'un utilisateur
Pour pouvoir créer un nouvel utilisateur, aller dans `Users > Users`
![[_Zabbix-1775636111254.jpeg]]
Cliquer sur "**Create user**"
![[_Zabbix-1775636185343.jpeg]]

Définir un nom d'utilisateur, un mot de passe et associer un groupe.
![[_Zabbix-1775636253807.jpeg]]
Dans l'onglet Permissions, cliquer sur "**Select**" et définir un rôle pour le nouvel utilisateur.
![[_Zabbix-1775636356301.jpeg]]
Cliquer sur "**Add**" pour créer l'utilisateur.

## 5. Ajouter un agent
### a. Installer un agent sous windows
Télécharger l'agent zabbix pour windows. [Télécharger ](https://www.zabbix.com/download_agents)
![[_Zabbix-1775640236767.jpeg]]

| ![[_Zabbix-1775640323201.jpeg]] | ![[_Zabbix-1775640331462.jpeg]] |
| ------------------------------- | ------------------------------- |
| ![[_Zabbix-1775640398276.jpeg]] | ![[_Zabbix-1775640421275.jpeg]] |
### b. Ajouter un hôte
Aller dans `Monitoring > Hosts` puis cliquer sur `Create Host`
![[_Zabbix-1775640651397.jpeg]]

|                                                                                                                                                                                                                                                                                                         |                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Sélectionner un Template en cliquant sur `Select` <br><br>Cliquer à nouveau sur `Select` pour sélectionner un groupe de template.<br>Sélectionner `Templates/Operating systems`                                                                                                                         | ![[_Zabbix-1775640911617.jpeg\|400]]![[_Zabbix-1775640969131.jpeg\|400]]![[_Zabbix-1775641026123.jpeg\|400]]![[_Zabbix-1775641427967.jpeg\|400]] |
| Sélectionner également un `Host Group`<br><br>Cliquer sur `Add`<br>Sélectionner `Agent` puis renseigner l'adresse IP du système à surveiller qui contient un agent Zabbix.<br><br><font color="#ff0000">Il est important de mettre le même port d'écoute que l'agent installé précédemment. </font><br> | ![[_Zabbix-1775641929996.jpeg]]                                                                                                                  |


## 6. Déclencheur (trigger)
Dans `Data collection > Hosts` , cliquer sur `Triggers` sur la ligne d'un hôte pour accéder a tous les déclencheurs actif.
![[_Zabbix-1775646786376.jpeg]]

### a. Activer/Désactiver un déclencheur
Repérer un déclencheur à activer ou désactiver, puis cliquer sur son statut pour changer l'état.
![[_Zabbix-1775646901129.jpeg]]

### b. Créer un nouveau déclencheur
Sur la page des déclencheur d'un hôte, cliquer sur `Create Trigger` 
![[_Zabbix-1775647198038.jpeg]]

Renseigner le nom du nouveau déclencheur, sa sévérité, puis pour simplifier la mise en place d'une expression, cliquer sur `Expression constructor` 
![[_Zabbix-1775647522877.jpeg]]
Cliquer sur `Edit` , puis sélectionner l'item pour la condition et la valeur maximale pour le valider la condition.
![[_Zabbix-1775647823842.jpeg]]
Une fois l'expression établie, cliquer sur `Add` pour l'ajouter à la liste des expression à prendre en compte pour le déclenchement.
![[_Zabbix-1775647964698.jpeg]]
![[_Zabbix-1775648009632.jpeg]]

Une fois le déclencheur configuré, cliquer sur `Add` pour l'ajouter la liste des déclencheurs.
![[_Zabbix-1775648064494.jpeg]]
