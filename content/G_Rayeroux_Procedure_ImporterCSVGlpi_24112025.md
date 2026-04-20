|                                    |
| ---------------------------------- |
|                                    |
| Importer des données dans GLPI     |
|                                    |
| Gautier RAYEROUX<br><br>24/11/2025 |

[Prérequis 2](#_Toc1353382111)

[Passer en super utilisateur 3](#_Toc1950162919)

[Télécharger l’archive “datainjection” 3](#_Toc1079792388)

[Installer le plugin 4](#_Toc1005436119)

[Créer un modèle d’injection 4](#_Toc422777154)

[Importer le fichier 6](#_Toc356911751)

# Prérequis

Vous devez au préalable avoir installé un serveur GLPI.

**Passer sous le profil du super utilisateur pour l’installation de GLPI pour avoir tous les privilèges.**

## Passer en super utilisateur

|                                                                                                                                                                           |                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Entrer la commande suivante pour passer en super utilisateur.<br><br>sudo su –<br><br>Entrer le mot de passe du compte super utilisateur puis appuyer sur « **Entrer** ». | ![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 9.png>) |

# Télécharger l’archive “datainjection”

Entrer la commande suivante pour télécharger l’archive du plugin datainjection.

|   |
|---|
|cd /var/www/html/glpi/plugins<br><br>wget https://github.com/pluginsGLPI/datainjection/releases/download/2.15.1/glpi-datainjection-2.15.1.tar.bz2<br><br>tar -xvf glpi-datainjection-2.15.1.tar.bz2|

# Installer le plugin

|   |   |
|---|---|
|Sur l’interface de GLPI, aller dans l’onglet “**Configuration**”, puis cliquer sur “**Plugins**”.<br><br>Sur la ligne “**Data Injection**”, activer le plugin.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 10.png>)|

# Créer un modèle d’injection

|   |   |
|---|---|
|Dans l’onglet “**Outils**”, cliquer sur “**Data Injection**”, puis cliquer sur l’icône “**Modèle**”<br><br>Cliquer sur “**Ajouter**”|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 11.png>)![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 12.png>)|
|Remplir les différents champs et choisir la catégorie de donner à injecter puis cliquer sur “**Ajouter**”.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 13.png>)|
|Sélectionner le fichier à injecter pour préparer le modèle.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 14.png>)|
|Effectuer les correspondances entre l’en-tête du fichier CSV aux champs de la catégorie à injecter, puis cliquer sur “**Sauvegarder**”.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 15.png>)|
|Aller dans l’onglet “**Validation**”, puis cliquer sur “**Valider le modèle**”.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 16.png>)|

# Importer le fichier

|   |   |
|---|---|
|Aller dans l’onglet “**Outils**”, puis cliquer sur “**Data Injection**”.<br><br>Sélectionner le modèle d’injection, puis sélectionner le fichier CSV qui contient toutes les données, puis cliquer sur “**Procéder à l’injection**”.|![G_Rayeroux_Procedure_ImporterCSVGlpi_24112025](<G_Rayeroux_Procedure_ImporterCSVGlpi_24112025 17.png>)|