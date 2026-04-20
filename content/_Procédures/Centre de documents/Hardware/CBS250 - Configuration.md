---
base: "[[_Centre de documents.base]]"
Créée par:
  - Gautier RAYEROUX
  - Eric JAMET
  - David GEMAIN
Catégorie:
  - Cisco
Date de création: 2025-01-08
---
**Auteur :** Gautier RAYEROUX, Eric JAMET, David GEMAIN  |  **Date :** 08/01/2025

---

## Prérequis

- 2 x Cisco CBS-250 24 ports
- 1 câble Serial
- 1 câble USB-to-Serial
- Putty

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 1.jpeg]]

---

## 1. Réinitialisation du switch

Appuyer sur le bouton **« reset »** au minimum **15 secondes** pour réinitialiser le switch aux paramètres de sortie d'usine.

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1).png]]

---

## 2. Connexion au switch via port COM

1. Sur Putty, entrer les paramètres suivants pour se connecter au switch :
   - 115200 bits per second
   - 8 data bits
   - No parity
   - 1 stop bit
   - No flow control

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 1.png]]

2. Utiliser les identifiants par défaut : **`cisco` / `cisco`**
3. Une fois connecté, changer le nom d'utilisateur et le mot de passe : **`admin` / `Afpa159*`**

> [!note]
> Les nombres consécutifs ne sont pas tolérés pour un mot de passe.

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 2.png]]

---

## 3. Créer un nouvel utilisateur d'administration

Passer le switch en mode configuration (`config`), puis exécuter :

```ios
username afpaIT password Afpa357*
```

---

## 4. Activation du SSH

1. Ajouter un mot de passe pour le mode privilégié :
```ios
enable password Afpa147*
```
![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 3.png]]
![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 4.png]]

2. Ajouter un nom de domaine au switch :
```ios
ip domain name scenario.lan
```
3. Activer le SSH :
```ios
ip ssh server
```
4. Générer une clé RSA :
```ios
crypto key generate rsa
```
5. Tester la connexion SSH avec Putty

---

## 5. Test de connexion via interface Web

Entrer l'adresse IP du switch dans le navigateur pour accéder à l'interface graphique, puis saisir les identifiants de connexion.

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 5.png]]

---

## 6. Créer les VLAN

### 6.1 Via l'interface graphique

Aller dans **VLAN Management** → **VLAN Settings**, puis cliquer sur **« Add »**.

Entrer le numéro de VLAN et son nom, puis valider.

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 6.png]]

### 6.2 Via ligne de commande

```ios
vlan 10 name Administratif
vlan 20 name Ventes
vlan 30 name IT
vlan 40 name WiFi
vlan 99 name Management
vlan 100 name Natif
```

---

## 7. Créer les interfaces SVI

```ios
Interface vlan 10
ip address 192.168.10.1 255.255.255.224
no shutdown
exit

Interface vlan 20
ip address 192.168.20.1 255.255.255.192
no shutdown
exit

Interface vlan 30
ip address 192.168.30.1 255.255.255.248
no shutdown
exit

Interface vlan 40
ip address 192.168.40.1 255.255.255.0
no shutdown
exit
```

---

## 8. Mise en place du trunk (GigabitEthernet 24)

1. Entrer en mode configuration et aller sur le port destiné au trunk :
```ios
interface ge24
```
![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 7.png]]

2. Définir le mode trunk, le VLAN natif et les VLANs autorisés :
```ios
switchport mode trunk
switchport trunk native vlan 100
switchport trunk allowed vlan 10,20,30,40,100
```

---

## 9. Assigner les VLAN aux ports

```ios
Interface range GigabitEthernet1-6
Switchport mode access
Switchport access vlan 10
no shutdown
exit

Interface range GigabitEthernet7-12
Switchport mode access
Switchport access vlan 20
no shutdown
exit

Interface range GigabitEthernet13-18
Switchport mode access
Switchport access vlan 30
no shutdown
exit

Interface GigabitEthernet19
Switchport mode access
Switchport access vlan 40
no shutdown
exit
```

---

## 10. Autoriser le management par ACL

Autoriser uniquement les utilisateurs du **VLAN 30** à se connecter en SSH et via interface Web :

```ios
management access-list mngt
permit vlan 30
exit
management access-class mngt
```

> [!warning]
> Il est important de retirer l'ACL actif avant d'y apporter une modification.

---

## 11. Copier la configuration sur le Switch 2

Récupérer la configuration avec :
```ios
show running-config
```

### Précautions avant de coller la configuration

- Désactiver le routage IP : `no ip routing`
- Supprimer les adresses IP des SVI (sauf management)
- Changer le hostname
- Changer l'adresse IP dans le VLAN Management :
  - Switch 1 : VLAN 99 → `192.168.99.1`
  - Switch 2 : VLAN 99 → `192.168.99.2`
- Ne pas mettre les mêmes identifiants
- Ajouter une adresse passerelle : `ip default-gateway`
- Recréer le 2ème compte utilisateur (`username`)

Puis coller la configuration sur le Switch 2 en mode configuration terminal avec `configure`.

---

## 12. Interdire l'accès aux passerelles SVI (SSH, HTTP, HTTPS)

```ios
! Créer l'ACL
ip access-list extended MGMT_ACCESS

! Autoriser l'accès du VLAN 30 au management de l'AP en VLAN 40
permit tcp 192.168.30.0 0.0.0.7 any 192.168.40.2 0.0.0.255 www ace-priority 1
permit tcp 192.168.30.0 0.0.0.7 any 192.168.40.2 0.0.0.255 443 ace-priority 2

! Bloquer SSH
deny tcp any any 192.168.10.1 0.0.0.31 22 ace-priority 10
deny tcp any any 192.168.20.1 0.0.0.63 22 ace-priority 11
deny tcp any any 192.168.30.1 0.0.0.7 22 ace-priority 12
deny tcp any any 192.168.40.1 0.0.0.255 22 ace-priority 13

! Bloquer HTTPS
deny tcp any any 192.168.10.1 0.0.0.31 443 ace-priority 20
deny tcp any any 192.168.20.1 0.0.0.63 443 ace-priority 21
deny tcp any any 192.168.30.1 0.0.0.7 443 ace-priority 22
deny tcp any any 192.168.40.1 0.0.0.255 443 ace-priority 23
deny tcp any any 192.168.1.254 0.0.0.255 443 ace-priority 24

! Bloquer HTTP
deny tcp any any 192.168.10.1 0.0.0.31 www ace-priority 40
deny tcp any any 192.168.20.1 0.0.0.63 www ace-priority 41
deny tcp any any 192.168.30.1 0.0.0.7 www ace-priority 42
deny tcp any any 192.168.40.1 0.0.0.255 www ace-priority 43
deny tcp any any 192.168.1.254 0.0.0.255 www ace-priority 44

permit ip any any ace-priority 60
exit
```

---

## 13. Bloquer le Wi-Fi vers le réseau Administratif

```ios
ip access-list extended Wifi
deny tcp 192.168.40.0 0.0.0.255 any 192.168.10.0 0.0.0.31 any ace-priority 1
permit ip any any ace-priority 100
```

---

## 14. Appliquer les ACL sur les VLAN

En mode configuration, sélectionner les VLAN où appliquer l'ACL :

```ios
Interface range vlan 10, vlan 20, vlan 30, vlan 40, vlan 100
Service-acl input MGMT_ACCESS
Exit

Interface vlan 40
Service-acl input Wifi
exit
```
