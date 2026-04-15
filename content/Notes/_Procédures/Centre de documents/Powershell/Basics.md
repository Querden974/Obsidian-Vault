---

---

### 🔹 **Déclaration de variables**

```powershell
$age = 25
$nom = "Gautier"
$estConnecte = $true
$pi = 3.14
$ville = "Angers"   # typage dynamique
```

---

### 🔹 **Structures de contrôle**

### Condition

```powershell
if ($age -gt 18) {
    Write-Output "Majeur"
}
else {
    Write-Output "Mineur"
}
```

### Switch

```powershell
switch ($jour) {
    "lundi" { Write-Output "Début de semaine" }
    default { Write-Output "Autre jour" }
}
```

### Boucles

```powershell
# for
for ($i = 0; $i -lt 5; $i++) {
    Write-Output $i
}

# while
$i = 0
while ($i -lt 5) {
    Write-Output $i
    $i++
}

# foreach
$fruits = @("pomme", "banane", "orange")
foreach ($fruit in $fruits) {
    Write-Output $fruit
}
```

---

### 🔹 **Fonctions**

```powershell
function Dire-Bonjour {
    param($nom)
    Write-Output "Bonjour $nom"
}

function Addition {
    param(
        [int]$a,
        [int]$b
    )
    return ($a + $b)
}
```

---

### 🔹 **Objets & Classes**

```powershell
class Personne {
    [string]$Nom
    [int]$Age

    [void]SePresenter() {
        Write-Output "Je m'appelle $($this.Nom) et j'ai $($this.Age) ans."
    }
}

# Utilisation
$p = [Personne]::new()
$p.Nom = "Gautier"
$p.Age = 26
$p.SePresenter()
```

---

### 🔹 **Listes et Tableaux**

```powershell
# Tableau
$notes = @(10, 15, 20)

# Liste dynamique (ArrayList)
$couleurs = New-Object System.Collections.ArrayList
$couleurs.Add("Rouge") | Out-Null
$couleurs.Add("Bleu")  | Out-Null
```

---

### 🔹 **Null safety**

```powershell
$nom = $null
if ($null -ne $nom) {
    Write-Output $nom.Length
}
```

---

### 🔹 **Try / Catch (gestion d’erreurs)**

```powershell
try {
    [int]::Parse("abc")
}
catch {
    Write-Output "Erreur : $($_.Exception.Message)"
}
```

---

### 🔹 **Script PowerShell (structure minimale)**

```powershell
Write-Output "Hello World!"

# Paramètres possibles
param(
    [string]$Utilisateur,
    [switch]$VerboseMode
)

Write-Output "Utilisateur : $Utilisateur"
```