---

---
## 🔹 **Modules**

### Import d’un module

```powershell
Import-Module ActiveDirectory
Import-Module Az
```

### Lister les modules installés

```powershell
Get-Module -ListAvailable
```

### Lister les commandes d’un module

```powershell
Get-Command -Module ActiveDirectory
```

### Installer un module depuis la PowerShell Gallery

```powershell
Install-Module -Name PSReadLine -Scope CurrentUser
```

---

## 🔹 **Cmdlets essentielles**

### Trouver une commande

```powershell
Get-Command *process*
```

### Aide intégrée

```powershell
Get-Help Get-Process
Get-Help Get-Process -Examples
```

### Objet & Propriétés

```powershell
Get-Service | Select-Object Name, Status
```

### Filtrer

```powershell
Get-Process | Where-Object { $_.CPU -gt 10 }
```

### Trier

```powershell
Get-Process | Sort-Object CPU -Descending
```

---

## 🔹 **Pipelines**

Le pipeline passe **des objets**, pas du texte :

```powershell
Get-Service | Where-Object { $_.Status -eq "Running" } | Select-Object Name
```

### Compter les éléments

```powershell
Get-Process | Measure-Object
```

### Pipeline avec export

```powershell
Get-Process | Export-Csv "process.csv" -NoTypeInformation
```

---

## 🔹 **Manipulation de fichiers & dossiers**

### Lister les fichiers

```powershell
Get-ChildItem "C:\Logs"
```

### Lire un fichier

```powershell
Get-Content "notes.txt"
```

### Écrire dans un fichier

```powershell
"Hello PowerShell" | Out-File "test.txt
```

### Ajouter du texte

```powershell
Add-Content -Path "test.txt" -Value "Nouvelle ligne"
```

### Copier / Déplacer / Supprimer

```powershell
Copy-Item "source.txt" "backup\source.txt"
Move-Item "source.txt" "archive\"
Remove-Item "old.txt"
```

### Créer un dossier

```powershell
New-Item -ItemType Directory -Path "C:\NouveauDossier"
```

---

## 🔹 **Manipulation JSON**

### Lire un fichier JSON

```powershell
$data = Get-Content "config.json" | ConvertFrom-Json
$data.Server.Name
```

### Modifier et réécrire un JSON

```powershell
$data.Version = "2.0"
$data | ConvertTo-Json | Set-Content "config.json"
```

---

## 🔹 **Manipulation REST API**

### GET

```powershell
Invoke-RestMethod -Uri "https://api.example.com/users"
```

### POST

```powershell
Invoke-RestMethod -Uri "https://api.example.com/login" -Method Post -Body @{
    user = "admin"
    pass = "1234"
}
```

---

## 🔹 **Gestion des Processus & Services**

### Processus

```powershell
Get-Process
Stop-Process -Name "notepad"
```

### Services

```powershell
Get-Service
Restart-Service -Name "Spooler"
```

---