---
tags:
  - proxmox
  - réseau
  - bridge
  - vlan
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Créée par:
  - Gautier RAYEROUX
Date de création: 2026-03-09
---
**Auteur :** Gautier RAYEROUX  |  **Date :** 09/03/2026

---

## Présentation des interfaces réseau

La configuration réseau de Proxmox est accessible via **nœud pve** → **System** → **Network**.

Par défaut après installation, deux interfaces sont présentes :

| Nom | Type | Description |
|---|---|---|
| `ens33` | Network Device | Interface physique (ou virtuelle VMware) |
| `vmbr0` | Linux Bridge | Bridge principal, lié à `ens33`, IP du nœud |

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 25.png]]

> [!note]+ Linux Bridge
> Un **Linux Bridge** est un switch virtuel qui relie les VMs à l'interface physique. `vmbr0` est le bridge par défaut créé à l'installation, il porte l'adresse IP du nœud Proxmox.

---

## 1. Créer un Linux Bridge supplémentaire

Un second bridge permet d'isoler des VMs dans un réseau dédié ou de gérer des VLANs.

1. Dans **System** → **Network**, cliquer sur **« Create »** → **« Linux Bridge »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 26.png]]

2. Renseigner les paramètres du nouveau bridge :
   - **Name :** `vmbr1`
   - **VLAN aware :** coché (pour permettre le trunking VLAN)
   - **Bridge ports :** `ens33`
   
   Cliquer sur **« Create »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 27.png]]

3. Cliquer sur **« Apply Configuration »** pour appliquer les changements réseau.

> [!warning]+ Attention
> Appliquer la configuration réseau peut couper brièvement la connectivité du nœud. Éviter de le faire en production sans planification.

---

## 2. Assigner un VLAN à une interface VM

Pour placer une VM dans un VLAN spécifique, il faut modifier son interface réseau depuis l'onglet **Hardware**.

1. Sélectionner la VM dans l'arborescence (ex. `100 (Debian13)`), puis aller dans **Hardware** et sélectionner **Network Device (net0)**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 28.png]]

2. Cliquer sur **« Edit »** et modifier :
   - **Bridge :** `vmbr1` (le bridge VLAN-aware)
   - **VLAN Tag :** `10`
   
   Cliquer sur **« OK »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 29.png]]

> [!info]+ VLAN Tag
> Le **VLAN Tag** configure le tag 802.1Q sur le port de la VM. Le bridge doit être **VLAN aware** pour que cela fonctionne.

---

## 3. Créer un Linux VLAN

Un Linux VLAN crée une interface virtuelle taguée sur une interface physique existante.

1. Dans **System** → **Network**, sélectionner le nœud **pve**, puis cliquer sur **« Create »** → **« Linux VLAN »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 30.png]]

2. Renseigner :
   - **Name :** `ens33.99` (convention : `<interface>.<vlan-id>`)
   - Le **VLAN Tag** est automatiquement détecté (`99`) depuis le nom
   
   Cliquer sur **« Create »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 31.png]]

---

## 4. Créer un bridge sur un Linux VLAN

Pour exposer le VLAN aux VMs, il faut créer un bridge dont le port est le Linux VLAN créé précédemment.

1. Créer un **Linux Bridge** (cf. section 1), avec :
   - **Name :** `vmbr99`
   - **Bridge ports :** `ens33.99`
   - **VLAN aware :** décoché (le trunking est géré par le VLAN sous-jacent)
   
   Cliquer sur **« OK »**

![[image/Attachments 3/G_Rayeroux_Procedure_Proxmox_09032026 32.png]]

2. Cliquer sur **« Apply Configuration »** pour appliquer.

---

## Récapitulatif — Types d'interfaces réseau Proxmox

| Type | Utilisation | Exemple |
|---|---|---|
| **Network Device** | Interface physique (uplink) | `ens33` |
| **Linux Bridge** | Switch virtuel pour les VMs | `vmbr0`, `vmbr1` |
| **Linux VLAN** | Interface taguée sur un uplink | `ens33.99` |
| **Linux Bond** | Agrégation de liens (HA réseau) | — |
| **OVS Bridge** | Bridge Open vSwitch (avancé) | — |
