---

---
# 🏛️ **🔹 Gestion des GPO (Group Policy Objects)**

> ⚠ Nécessite le module GroupPolicy

```powershell
Import-Module GroupPolicy
```

---

## 🔹 **Lister les GPO existantes**

```powershell
Get-GPO -All
```

---

## 🔹 **Créer une GPO**

```powershell
New-GPO -Name "GPO_Sécurité_Serveurs"
```

---

## 🔹 **Lier une GPO à une OU**

```powershell
New-GPLink -Name "GPO_Sécurité_Serveurs" -Target "OU=Serveurs,DC=entreprise,DC=local"
```

---

## 🔹 **Sauvegarder une GPO**

```powershell
Backup-GPO -Name "GPO_Sécurité_Serveurs" -Path "C:\Backup\GPO"
```

---

## 🔹 **Restaurer une GPO**

```powershell
Restore-GPO -Name "GPO_Sécurité_Serveurs" -Path "C:\Backup\GPO"
```

---

## 🔹 **Exporter / Importer les paramètres d’une GPO**

```powershell
# Export
Backup-GPO -Name "GPO_VPN" -Path "C:\Backup"

# Import dans une nouvelle GPO
Import-GPO -BackupGpoName "GPO_VPN" `
           -TargetName "GPO_VPN_New" `
           -Path "C:\Backup"
```

---

## 🔹 **Récupérer les paramètres principaux d’une GPO**

```powershell
Get-GPOReport -Name "GPO_VPN" -ReportType Html -Path "C:\Reports\GPO_VPN.html"
```

---

## 🔹 **Forcer la mise à jour des stratégies**

```powershell
gpupdate /force
```

---

## 🔹 **Lister les paramètres appliqués sur une machine**

```powershell
Get-GPResultantSetOfPolicy -ReportType Html -Path "C:\RSoP.html"
```

---

# 🧱 **🔹 Sécurité GPO : permissions**

## Ajouter un droit à un groupe

```powershell
Set-GPPermission -Name "GPO_Sécurité" -TargetName "Admins" -TargetType Group -PermissionLevel GpoEdit
```

## Voir les permissions d’une GPO

```powershell
Get-GPPermission -Name "GPO_Sécurité" -All
```