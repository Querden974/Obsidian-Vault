---

---
## Installation du rôle Active Directory

### Installer les rôles nécessaires

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

Vérification :

```powershell
Get-WindowsFeature AD-Domain-Services
```

---

## Promotion du serveur en contrôleur de domaine

### Lancer l’installation de la forêt Active Directory

```powershell
Install-ADDSForest `
-DomainName "mondomaine.local" `
-DomainNetbiosName "MONDOMAINE" `
-InstallDNS `
-NoRebootOnCompletion
```

Paramètres importants :

- `mondomaine.local` : nom DNS du domaine
- `MONDOMAINE` : nom NetBIOS
- DNS installé automatiquement

### 4.2 Définir le mot de passe DSRM

Un mot de passe de récupération **sera demandé**

---

## Redémarrage du serveur

```powershell
Restart-Computer
```

Après redémarrage :

- Le serveur est **Contrôleur de Domaine**
- Le DNS est fonctionnel
- L’Active Directory est opérationnel

---

## Vérifications post-installation

### Vérifier l’état du domaine

```powershell
Get-ADDomain
```

### 6.2 Vérifier le contrôleur de domaine

```powershell
Get-ADDomainController
```

### 6.3 Vérifier le service DNS

```powershell
Get-Service DNS
```