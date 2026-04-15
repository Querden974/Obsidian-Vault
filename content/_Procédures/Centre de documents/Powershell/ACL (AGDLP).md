---

---
# 🧩 **🔹 Modèles d’appartenance Active Directory : AGDLP / AGUDLP**

---

## 🏷️ **Qu’est-ce que le modèle AGDLP ?**

**A**ccounts → **G**lobal Groups → **D**omain **L**ocal Groups → **P**ermissions

➡ **Modèle recommandé en environnement Windows Server**

➡ Utile pour structurer proprement les droits sur les ressources.

---

# 🧾 **🔹 Mise en place AGDLP en PowerShell**

---

## 1️⃣ **Créer les groupes**

### Groupe Global (GG – regroupe les utilisateurs)

```powershell
New-ADGroup -Name "GG_Compta" -GroupScope Global -GroupCategory Security -Path "OU=Groupes,OU=Compta,DC=entreprise,DC=local"
```

### Groupe Local de Domaine (GL – appliqué sur les ressources)

```powershell
New-ADGroup -Name "DL_Compta_ServeurFiles_RW" -GroupScope DomainLocal -GroupCategory Security -Path "OU=Groupes_DL,DC=entreprise,DC=local"
```

---

## 2️⃣ **Ajouter les utilisateurs au groupe Global**

```powershell
Add-ADGroupMember -Identity "GG_Compta" -Members "jdupont", "mdupuis"
```

---

## 3️⃣ **Ajouter le groupe Global au groupe Local**

```powershell
Add-ADGroupMember -Identity "DL_Compta_ServeurFiles_RW" -Members "GG_Compta"
```

---

## 4️⃣ **Appliquer les permissions sur un dossier**

```powershell
$acl = Get-Acl "D:\Partage\Compta"
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule("DL_Compta_ServeurFiles_RW","Modify","ContainerInherit, ObjectInherit","None","Allow")
$acl.AddAccessRule($rule)
Set-Acl "D:\Partage\Compta" $acl
```