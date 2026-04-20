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

Avant d'activer le portail captif, s'assurer que :
- Le **DHCP du VLAN 40** est géré par pfSense (voir [[_Procédures/Centre de documents/pfSense/DHCP]])
- La passerelle par défaut du VLAN 40 pointe vers pfSense
- Le serveur **RADIUS** est configuré si l'authentification RADIUS est souhaitée (voir [[RADIUS]])

> [!note]
> Le VLAN 40 (Wi-Fi) est exclusivement routé par pfSense. Les VLAN 10, 20 et 30 continuent d'être routés par le switch Cisco CBS250.

---

## 1. Créer une zone de portail captif

1. Aller dans **Services** → **Captive Portal**
2. Cliquer sur **« Add »**
3. Définir le **nom de la zone**
4. Définir une **description**
5. Cliquer sur **« Save and Continue »**

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 106.png]]

---

## 2. Configurer le portail

1. **Activer** le portail captif
2. Sélectionner les **interfaces concernées** (ex. : VLAN 40 / Wifi)

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 107.png]]

3. Modifier les paramètres suivants pour personnaliser les images et le logo du portail :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 108.png]]

4. Sélectionner un **mode d'authentification**
5. Choisir le **serveur d'authentification** (RADIUS ou local)
6. Choisir un second serveur d'authentification si besoin
7. **Sauvegarder**

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 109.png]]

---

## 3. Résultat

Les appareils connectés au réseau Wi-Fi ouvrent automatiquement la page du portail captif avant d'accéder à Internet.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 110.png]]
![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 3.jpeg]]
