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

## Identifiants RADIUS

| Paramètre | Valeur |
|---|---|
| Shared Secret | Afpa123* |
| NAS login | admin / Afpa123* |

---

## 1. Installation du package FreeRADIUS

1. Aller dans **System** → **Package Manager** → **Available Packages**
2. Rechercher **« freeradius3 »** et l'installer

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 23.png]]

3. Une fois installé, aller dans **Services** → **FreeRADIUS**

---

## 2. Configurer l'interface d'écoute

Dans l'onglet **« Interfaces »**, cliquer sur **« Add »** :

1. Entrer l'adresse IP de l'interface d'écoute
2. Définir le port d'écoute
3. Définir le type d'interface
4. Sélectionner la version IP (IPv4 / IPv6)
5. Définir une description

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 111.png]]

---

## 3. Créer un NAS / Client

Dans l'onglet **« NAS/Clients »**, cliquer sur **« Add »** :

1. Entrer l'adresse IP du NAS RADIUS avec son CIDR
2. Sélectionner la version IPv4 ou IPv6
3. Donner un nom au client
4. Définir un **secret partagé** (Shared Secret)
5. Définir le login du NAS
6. Définir le mot de passe du NAS
7. Donner une description

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 112.png]]

---

## 4. Créer un utilisateur RADIUS

Dans l'onglet **« Users »**, cliquer sur **« Add »** :

1. Définir le nom d'utilisateur
2. Définir un mot de passe
3. Sauvegarder

> [!note]
> Ce compte sera utilisé pour **OpenVPN** et le **portail captif**.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 113.png]]

---

## 5. Créer un serveur d'authentification

Aller dans **System** → **User Manager** → onglet **« Authentication Servers »**, puis cliquer sur **« Add »** :

1. Donner un nom descriptif au serveur
2. Sélectionner le type **« RADIUS »**
3. Sélectionner le protocole **« MS-CHAPv2 »**
4. Renseigner l'adresse IP du NAS RADIUS (ou `127.0.0.1` si local)
5. Renseigner le **shared secret** défini dans NAS/Client
6. Sélectionner le **RADIUS NAS IP Attribute** sur **« LAN »**
7. Sauvegarder

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 114.png]]

---

## 6. Tester la connexion RADIUS

Aller dans **Diagnostics** → **Authentication** pour tester les identifiants d'un utilisateur RADIUS.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 115.png]]

---

## 7. Utiliser RADIUS avec OpenVPN

Dans la configuration du serveur **OpenVPN**, sélectionner le mode **« Remote Access (User Auth) »** pour utiliser la base d'utilisateurs RADIUS.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 116.png]]
