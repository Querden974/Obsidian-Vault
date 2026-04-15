---
tags:
  - powershell
  - scripting
  - windows
  - index
base: "[[_Centre de documents.base]]"
Catégorie:
  - Windows
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T00:00:00
---

# PowerShell — Centre de Ressources

> [!abstract] Présentation
> **PowerShell** est un shell en ligne de commande et un langage de script orienté objet développé par Microsoft. Basé sur le framework .NET, il manipule des **objets** plutôt que du texte brut, ce qui le rend particulièrement puissant pour l'automatisation de tâches Windows, l'administration Active Directory et la gestion des infrastructures.

---

## Fondamentaux du langage

> [!example] Bases
> ### [[Notes/_Procédures/Centre de documents/Powershell/Basics]]
> Syntaxe essentielle pour écrire ses premiers scripts PowerShell.
>
> - Variables et types (`$var`, `[int]`, `[string]`…)
> - Conditions : `if / else`, `switch`
> - Boucles : `for`, `while`, `foreach`
> - Fonctions avec `param()`
> - Classes et objets (.NET)
> - Gestion d'erreurs : `try / catch`

> [!example] Opérateurs de comparaison
> ### [[Opérateurs de comparaison PowerShell]]
> PowerShell utilise des mots-clés à la place des symboles classiques.
>
> | Opérateur | Rôle |
> |---|---|
> | `-eq` / `-ne` | Égalité / différence |
> | `-gt` / `-lt` | Supérieur / inférieur |
> | `-like` / `-match` | Wildcard / regex |
> | `-and` / `-or` / `-not` | Logique booléenne |
> | `-contains` / `-in` | Appartenance à une collection |
> | `-is` / `-isnot` | Test de type .NET |

---

## Scripting avancé

> [!info] Fonctionnalités avancées
> ### [[Advanced]]
> Aller plus loin avec les modules, le pipeline objet et les interactions système.
>
> - Modules : `Import-Module`, `Install-Module`, `Get-Command`
> - Cmdlets clés : `Where-Object`, `Sort-Object`, `Select-Object`
> - Pipeline objet : filtrage, comptage, export CSV
> - Fichiers & dossiers : `Get-ChildItem`, `Get-Content`, `Copy-Item`
> - Manipulation JSON : `ConvertFrom-Json`, `ConvertTo-Json`
> - REST API : `Invoke-RestMethod` (GET / POST)
> - Processus & services : `Get-Process`, `Restart-Service`

---

## Active Directory & Sécurité

> [!note] Active Directory
> ### [[Active Directory]]
> Administrer les objets AD (utilisateurs, groupes, ordinateurs).
>
> - `Get-ADUser` / `New-ADUser` / `Set-ADUser`
> - Réinitialisation de mot de passe
> - `Add-ADGroupMember` / `Get-ADGroupMember`
> - `Get-ADComputer`
>
> ```powershell
> Import-Module ActiveDirectory
> ```

> [!note] GPO
> ### [[GPO]]
> Gérer les stratégies de groupe via PowerShell.
>
> - `Get-GPO` / `New-GPO`
> - Lier une GPO à une OU : `New-GPLink`
> - Sauvegarder / Restaurer : `Backup-GPO`
> - Rapport HTML : `Get-GPOReport`
> - `gpupdate /force`
>
> ```powershell
> Import-Module GroupPolicy
> ```

> [!note] ACL — AGDLP
> ### [[ACL (AGDLP)]]
> Structurer les droits sur les ressources selon le modèle AGDLP.
>
> **A**ccounts → **G**lobal → **D**omain **L**ocal → **P**ermissions
>
> - Créer groupes Global et DomainLocal
> - Imbriquer les groupes
> - Appliquer les ACL avec `Set-Acl`

---

## Administration en mode Core (sans GUI)

> [!warning] Active Directory (Core)
> ### [[Active directory (Core)]]
> Installer et promouvoir un contrôleur de domaine sans interface graphique.
>
> - `Install-WindowsFeature AD-Domain-Services`
> - `Install-ADDSForest` (forêt + DNS intégré)
> - Définition du mot de passe DSRM
> - `Get-ADDomain` / `Get-ADDomainController`

> [!warning] Serveur DHCP (Core)
> ### [[Serveur DHCP (Core)]]
> Déployer un serveur DHCP complet en ligne de commande.
>
> - `Add-DhcpServerv4Scope` — plage d'adresses
> - Options : passerelle (3), DNS (6), suffixe DNS (15)
> - `Set-DhcpServerv4Scope` — activer l'étendue
> - `Get-DhcpServerv4Scope` — vérification

---

## Utilitaires

> [!tip] Forcer la langue française sur Windows
> ### [[Forcer la langue Française sur Windows]]
> Appliquer les paramètres régionaux français en une seule exécution.
>
> ```powershell
> Set-WinUILanguageOverride -Language fr-FR
> Set-WinUserLanguageList -LanguageList fr-FR -Force
> Set-WinSystemLocale fr-FR
> Set-Culture fr-FR
> Set-WinHomeLocation -GeoId 84
> ```

---

## Rappels essentiels

| Cmdlet | Description |
|---|---|
| `Get-Help <cmdlet>` | Afficher l'aide d'une commande |
| `Get-Command *mot*` | Rechercher une commande |
| `Get-Module -ListAvailable` | Lister les modules disponibles |
| `$_` | Variable automatique de pipeline |
| `Write-Output` / `Write-Host` | Afficher du texte |
| `Export-Csv` | Exporter vers un fichier CSV |
| `Invoke-Command` | Exécuter une commande à distance |
| `Set-ExecutionPolicy RemoteSigned` | Autoriser l'exécution des scripts |

---

> [!tip] Progression recommandée
> 1. Maîtriser la syntaxe de base → [[Notes/_Procédures/Centre de documents/Powershell/Basics]]
> 2. Comprendre les opérateurs → [[Opérateurs de comparaison PowerShell]]
> 3. Explorer le pipeline et les modules → [[Advanced]]
> 4. Administrer Active Directory → [[Active Directory]] et [[GPO]]
> 5. Gérer les droits avec le modèle AGDLP → [[ACL (AGDLP)]]
> 6. Déployer des services en mode Core → [[Active directory (Core)]] et [[Serveur DHCP (Core)]]
