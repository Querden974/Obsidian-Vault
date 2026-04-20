---
base: "[[_Centre de documents.base]]"
Créée par:
  - Gautier RAYEROUX
Catégorie:
  - Linux
Date de création: 2026-01-15
---
**Auteur :** Gautier RAYEROUX  |  **Date :** 15/01/2026

---

## Présentation

La haute disponibilité pfSense repose sur le protocole **CARP** (Common Address Redundancy Protocol). Un pfSense **MAÎTRE** et un pfSense **ESCLAVE** partagent des adresses IP virtuelles. En cas de panne du MAÎTRE, l'ESCLAVE prend le relai automatiquement.

---

## Prérequis

Créer une seconde machine virtuelle destinée à être le pfSense **ESCLAVE**.

Pour les 2 VMs, ajouter une carte réseau dédiée au protocole CARP, puis paramétrer les interfaces en **Static IPv4**.

> [!note]
> Changer l'adresse LAN de chaque pfSense pour éviter les conflits.

| | MAÎTRE | ESCLAVE |
|---|---|---|
| LAN | 192.168.152.200 | 192.168.152.201 |
| DMZ | 192.168.100.1 | 192.168.100.2 |
| CARP | 192.168.2.1 | 192.168.2.2 |

---

## 1. Créer les Virtual IPs

Dans **Firewall** → **Virtual IPs**, ajouter 2 nouvelles adresses virtuelles (une par interface partagée).

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 67.png]]
![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 68.png]]

Répéter les opérations sur le pfSense **ESCLAVE**.

---

## 2. Modifier le DHCP (si activé)

Aller dans **Services** → **DHCP Server**, sélectionner **LAN**, puis modifier la **Gateway** en plaçant l'adresse virtuelle CARP créée précédemment, puis sauvegarder.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 69.png]]

---

## 3. Activer la haute disponibilité (MAÎTRE)

Depuis le pfSense **MAÎTRE**, aller dans **System** → **High Availability** et renseigner la configuration de synchronisation.

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 70.png]]
![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 71.png]]

---

## 4. Paramétrer les règles de pare-feu pour la haute disponibilité

### 4.1 NAT Outbound

Ajouter une nouvelle règle **NAT Outbound** pour gérer le NAT sortant sur l'interface WAN :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 72.png]]
![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 73.png]]

### 4.2 Règle pour l'interface CARP

Dans les règles de Firewall, ajouter une règle pour l'interface CARP :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 74.png]]

### 4.3 Règle pour le protocole XML-RPC

Créer une règle pour le protocole XML-RPC (synchronisation de la configuration entre MAÎTRE et ESCLAVE) :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 75.png]]

### 4.4 Règle pour les flux internes CARP

Créer une règle pour les flux au sein du réseau CARP :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 76.png]]

---

## 5. Définir la passerelle par défaut

1. Aller dans **System** → **Routing**, créer une Gateway avec la passerelle fournie par le FAI, puis la sélectionner dans **« Default gateway IPv4 »**

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 77.png]]

2. Dans les règles de l'interface LAN, modifier la règle **« Default allow LAN to any rule »**, accéder aux paramètres avancés, puis changer la Gateway

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 78.png]]

---

## 6. Vérifier la haute disponibilité

Aller dans **Status** → **CARP (failover)** pour vérifier le statut.

| MAÎTRE | ESCLAVE |
|---|---|
| ![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 79.png]] | ![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 80.png]] |

Sur le pfSense ESCLAVE, vérifier que la configuration a bien été synchronisée depuis le MAÎTRE :

![[image/Attachments 3/G_Rayeroux_Procedure_PriseEnMainPfsense_15012026(1) 81.png]]

> [!note]
> Pour tester la bascule, désactiver temporairement CARP sur le pfSense **MAÎTRE**. Le statut CARP de l'**ESCLAVE** doit passer en **MASTER** automatiquement. Une fois le MAÎTRE remis en ligne, il reprend son rôle.
