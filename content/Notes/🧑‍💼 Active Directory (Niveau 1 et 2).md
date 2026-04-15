---

---
## 🗂️ Interface Graphique (RSAT / ADUC)

| Action | Où / Comment |
| --- | --- |
| **Créer un utilisateur** | ADUC → clic droit sur une OU → "Nouveau > Utilisateur" |
| **Réinitialiser un mot de passe** | Clic droit sur l'utilisateur → "Réinitialiser le mot de passe" |
| **Déverrouiller un compte** | Clic droit > "Propriétés" > onglet "Compte" > cocher "Déverrouiller" |
| **Déplacer un utilisateur** | Glisser-déposer dans une autre OU ou clic droit > "Déplacer" |
| **Ajouter à un groupe** | Onglet "Membre de" > "Ajouter" |
| **Vérifier les groupes d’un utilisateur** | Propriétés > "Membre de" |
| **Rechercher un utilisateur ou une machine** | ADUC > Clic droit sur le domaine > "Rechercher" |
| **Créer un groupe de sécurité** | OU > "Nouveau > Groupe" |
| **Renommer un compte** | Clic droit > "Renommer" (évite de recréer le compte) |

---

## 💻 Commandes PowerShell utiles avec Active Directory

⚠️ Il faut que le module **ActiveDirectory** soit installé (RSAT) et lancé avec les droits suffisants.

<!-- Column 1 -->
### 🔍 Recherche / Infos

```powershell
Get-ADUser -Identity jdupont
Get-ADUser -Filter "Name -like '*dupont*'"
Get-ADUser jdupont -Properties LockedOut, Enabled, PasswordLastSet
Get-ADComputer -Filter "Name -like '*PC*'"  # Rechercher un PC
```

### 🔐 Mot de passe

```powershell
Set-ADAccountPassword -Identity jdupont -Reset -NewPassword (ConvertTo-SecureString "NouveauMDP123!" -AsPlainText -Force)
```

### 🧾 Autres utiles

```powershell
Get-ADUser jdupont | Format-List *  # Affiche toutes les propriétés
Get-ADUser jdupont -Properties PasswordLastSet | Select-Object Name, PasswordLastSet
```

<!-- Column 2 -->
### 👤 Gestion des utilisateurs

```powershell
New-ADUser -Name "Jean Dupont" -GivenName Jean -Surname Dupont -SamAccountName jdupont -UserPrincipalName jdupont@domaine.local -Path "OU=Utilisateurs,DC=domaine,DC=local" -AccountPassword (Read-Host -AsSecureString "Mot de passe") -Enabled $true

Set-ADUser jdupont -Title "Technicien" -Department "IT"
Set-ADUser jdupont -AccountExpirationDate "2025-12-31"

Unlock-ADAccount -Identity jdupont
Enable-ADAccount -Identity jdupont
Disable-ADAccount -Identity jdupont
Remove-ADUser -Identity jdupont  # ⚠️ Supprime le compte
```

### 👥 Groupes

```powershell
Add-ADGroupMember -Identity "IT-Support" -Members jdupont
Remove-ADGroupMember -Identity "IT-Support" -Members jdupont
Get-ADGroupMember -Identity "IT-Support"
```

✅ Pour aller plus loin :

- Explorer les GPO (Group Policy Objects)
- Comprendre la réplication AD (sites, services)
- Auditer les connexions utilisateurs via `eventvwr`
