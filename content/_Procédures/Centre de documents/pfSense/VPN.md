---
base: "[[_Centre de documents.base]]"
Créée par:
  - Gautier RAYEROUX
  - Eric JAMET
  - David GEMAIN
Catégorie:
  - Linux
Date de création: 2025-01-08
---
**Auteur :** Gautier RAYEROUX, Eric JAMET, David GEMAIN  |  **Date :** 08/01/2025

---

## Prérequis

- Le package **openvpn-client-export** doit être installé (voir étape 4)
- Le serveur **RADIUS** doit être configuré si l'authentification RADIUS est souhaitée (voir [[RADIUS]])

---

## 1. Création de l'autorité de certification (CA)

1. Aller dans **System** → **Certificates** → onglet **« Authorities »**
2. Créer un nouveau certificat d'autorité

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 89.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 90.png]]

---

## 2. Créer le certificat serveur

1. Aller dans **System** → **Certificates** → onglet **« Certificates »**
2. Cliquer sur **« Add/Sign »**
3. Sélectionner la CA créée à l'étape précédente

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 91.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 92.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 93.png]]

---

## 3. Créer les utilisateurs locaux (authentification locale)

Si vous n'utilisez pas RADIUS, créer les utilisateurs directement dans pfSense :

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 94.png]]

---

## 4. Configurer le serveur OpenVPN

1. Aller dans **VPN** → **OpenVPN** → onglet **« Servers »**, puis cliquer sur **« Add »**

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 95.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 96.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 97.png]]

### Paramètres clés

| Paramètre | Description |
|---|---|
| **IPv4 Tunnel Network** | Réseau VPN attribué aux clients — chaque client connecté reçoit une IP dans ce réseau |
| **Redirect IPv4 Gateway** | Si coché : **full tunnel** (tout le trafic passe dans le VPN). Sinon : **split-tunnel** |
| **IPv4 Local network** | Réseaux LAN accessibles via le VPN (séparer par des virgules si plusieurs) |
| **Concurrent connections** | Nombre de connexions VPN simultanées autorisées |

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 98.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 99.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 100.png]]

---

## 5. Exporter la configuration OpenVPN pour le client

1. Aller dans **System** → **Package Manager** → **Available Packages**
2. Rechercher **« openvpn-client-export »** et l'installer

> [!note]
> Si aucun paquet n'est trouvé, passer en mode Shell sur l'interface CLI et exécuter :
> ```plain text
> pkg update
> ```

3. Une fois installé, aller dans **VPN** → **OpenVPN** → **Client Export**

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 101.png]]
![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 102.png]]

4. Cliquer sur **« Save as default »**
5. Dans la section **OpenVPN Clients**, cliquer sur **« Archive »** pour télécharger l'archive de configuration

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 103.png]]

6. Installer la configuration dans le dossier `config` sur le PC client

---

## 6. Créer les règles de pare-feu pour OpenVPN

### 6.1 Autoriser le flux OpenVPN entrant (WAN)

Ajouter une nouvelle règle dans **Firewall** → **Rules** → **WAN** :

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 104.png]]

### 6.2 Autoriser les flux vers les ressources (interface OpenVPN)

Ajouter les règles en fonction des ressources à rendre disponibles via VPN.

Exemple — autoriser le RDP depuis OpenVPN :

Ajouter une règle dans **Firewall** → **Rules** → **OpenVPN** :

![[G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 105.png]]

---

## 7. Test de la connexion RDP via VPN

![[Installation_CISCO_CSB250_08012025(1) 36.png]]
![[Installation_CISCO_CSB250_08012025(1) 37.png]]
![[Installation_CISCO_CSB250_08012025(1) 38.png]]
![[Installation_CISCO_CSB250_08012025(1) 39.png]]
