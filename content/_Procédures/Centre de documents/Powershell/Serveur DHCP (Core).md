---

---
# Installer le rôle

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

# Créer une étendue

## 1.Définir la plage d’adresse

```powershell
Add-DhcpServerv4Scope -Name "Scope1" -StartRange 192.168.1.50 -EndRange 192.168.1.100 -SubnetMask 255.255.255.0
```

## 2.Définir une passerelle par défaut

```powershell
Set-DhcpServerv4OptionDefinition -OptionId 3 -DefaultValue 192.168.1.254
```

## 3.Définir une adresse DNS

```powershell
Set-DhcpServerv4OptionDefinition -OptionId 6 -DefaultValue 192.168.1.253
```

## 4.Définir un suffixe DNS

```powershell
Set-DhcpServerv4OptionDefinition -OptionId 15 -DefaultValue it-connect.fr
```

> [!note]+ ### Tableau des options
> | Name | OptionId | Type |
> | --- | --- | --- |
> | Itinéraires statiques sans classe | 121 | BinaryData |
> | Masque de sous-réseau | 1 | IPv4Address |
> | Décalage de temps | 2 | DWord |
> | Serveur de temps | 4 | IPv4Address |
> | Serveurs de noms | 5 | IPv4Address |
> | Serveurs de connexion | 7 | IPv4Address |
> | Serveurs de cookies | 8 | IPv4Address |
> | Serveurs LPR | 9 | IPv4Address |
> | Serveurs Impress | 10 | IPv4Address |
> | Serveurs de recherche des ressources | 11 | IPv4Address |
> | Nom d'hôte | 12 | String |
> | Taille du fichier de démarrage | 13 | Word |
> | Fichier de vidage Merit | 14 | String |
> | Serveur d'échange | 16 | IPv4Address |
> | Chemin d'accès de la racine | 17 | String |
> | Chemin d'accès des extensions | 18 | String |
> | Transfert sur la couche IP | 19 | Byte |
> | Routage de source non locale | 20 | Byte |
> | Masques de filtre de strat?gie | 21 | IPv4Address |
> |   |   |   |
> | Durée de vie IP par défaut | 23 | Byte |
> |   |   |   |
> | Table plateau MTU de chemin | 25 | Word |
> | Option MTU | 26 | Word |
> | Tous les sous-réseaux sont locaux | 27 | Byte |
> | Adresse de diffusion | 28 | IPv4Address |
> | Découvrir le masque | 29 | Byte |
> | Option de fourniture de masque | 30 | Byte |
> | Découvrir les routeurs | 31 | Byte |
> | Adresse de sollicitations de routeur | 32 | IPv4Address |
> | Option d'itinéraire statique | 33 | IPv4Address |
> | Encapsulation des codes de fin | 34 | Byte |
> | Dépassement de délai de cache ARP | 35 | DWord |
> |   |   |   |
> | Durée de vie TCP par défaut | 37 | Byte |
> | Intervalle de conservation de connexion active | 38 | DWord |
> | Octet de conservation de connexion active | 39 | Byte |
> | Nom de domaine NIS | 40 | String |
> | Serveurs NIS | 41 | IPv4Address |
> | Serveurs NTP | 42 | IPv4Address |
> | Informations spécifiques au fournisseur | 43 | BinaryData |
> | Serveurs WINS/NBNS | 44 | IPv4Address |
> | NBDD NetBIOS sur TCP/IP | 45 | IPv4Address |
> | Type de noeud WINS/NBT | 46 | Byte |
> | Identificateur d'étendue NetBIOS | 47 | String |
> | Police système X Windows | 48 | IPv4Address |
> | Affichage sur système X Windows | 49 | IPv4Address |
> | Bail | 51 | DWord |
> | Durée de renouvellement (T1) | 58 | DWord |
> | Durée de reliaison (T2) | 59 | DWord |
> | Nom de domaine NIS+ | 64 | String |
> | Serveurs NIS+ | 65 | IPv4Address |
> | Nom d'hôte du serveur de démarrage | 66 | String |
> | Nom du fichier de démarrage | 67 | String |
> | Agents locaux IP mobiles | 68 | IPv4Address |
> | Serveurs SMTP (Simple Mail Transport Protocol) | 69 | IPv4Address |
> | Serveurs POP3 (Post Office Protocol) | 70 | IPv4Address |
> | Serveurs NNTP (Network News Transport Protocol) | 71 | IPv4Address |
> | Serveurs WWW (World Wide Web) | 72 | IPv4Address |
> | Serveurs Finger | 73 | IPv4Address |
> | Serveurs IRC (Internet Relay Chat) | 74 | IPv4Address |
> | Serveurs StreetTalk | 75 | IPv4Address |
> | Serveurs STDA (StreetTalk Directory Assistance) | 76 | IPv4Address |
> | Routeur | 3 | IPv4Address |
> | Serveurs DNS | 6 | IPv4Address |
> | Nom de domaine DNS | 15 | String |

# Activer une étendue

```powershell
Set-DhcpServerv4Scope -ScopeId 172.16.0 -Name "Scope1" -State Active
```

# Visualiser les étendues

```powershell
Get-DhcpServerv4Scope
```