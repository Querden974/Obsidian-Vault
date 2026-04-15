---

---
# 🧩 **Active Directory (AD)**

> Nécessite :

```powershell
Import-Module ActiveDirectory
```

---

## 🔹 **Rechercher un utilisateur**

```powershell
Get-ADUser -Identity "gautier" -Properties *
```

### Rechercher par filtre

```powershell
Get-ADUser -Filter { Department -eq "IT" } -Properties Department
```

---

### Vérifier un objet existant

```powershell
$existingUser = Get-ADUser -Filter "SamAccountName -eq '$($u.samAccountName)'" -ErrorAction SilentlyContinue
```

## 🔹 **Créer un utilisateur**

```powershell
New-ADUser `
    -Name "Jean Dupont" `
    -SamAccountName "jdupont" `
    -UserPrincipalName "jdupont@entreprise.local" `
    -Path "OU=Utilisateurs,DC=entreprise,DC=local" `
    -AccountPassword (Read-Host -AsSecureString "Password") `
    -Enabled $true
```

---

## 🔹 **Modifier un utilisateur**

```powershell
Set-ADUser -Identity "jdupont" -Department "RH"
```

---

## 🔹 **Réinitialiser un mot de passe**

```powershell
Set-ADAccountPassword -Identity "jdupont" -Reset -NewPassword (Read-Host -AsSecureString)
```

---

## 🔹 **Gérer les groupes**

### Ajouter un utilisateur à un groupe

```powershell
Add-ADGroupMember -Identity "Admins" -Members "jdupont"
```

### Voir les membres

```powershell
Get-ADGroupMember -Identity "Admins"
```

---

## 🔹 **Rechercher un ordinateur**

```powershell
Get-ADComputer -Filter * -Properties OperatingSystem
```