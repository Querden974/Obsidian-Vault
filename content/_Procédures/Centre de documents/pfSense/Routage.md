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
## 1. Création des routes statiques VLAN

Aller dans **System** → **Routing** → **Static Routes** pour créer les routes vers chaque VLAN.

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 17.png]]

---

## 2. Création des gateways

1. Aller dans **System** → **Routing** → **Gateways**
2. Créer les gateways pour les interfaces **LAN** et **WAN**

![[image/Attachments 3/Installation_CISCO_CSB250_08012025(1) 18.png]]

> [!warning]
> Laisser la **default gateway IPv4** en mode **« Automatic »**.
