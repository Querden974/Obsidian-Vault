---
tags:
  - cisco
  - réseau
  - packet-tracer
  - index
Catégorie:
  - Réseau
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T00:00:00
base: "[[_Centre de documents.base]]"
---

# Cisco Packet Tracer — Centre de Ressources

> [!abstract] Présentation
> **Cisco Packet Tracer** est un simulateur réseau permettant de concevoir, configurer et dépanner des infrastructures Cisco sans matériel physique. Les équipements sont configurés via le **CLI IOS**, dont les commandes sont organisées en modes successifs :
>
> ```
> > enable  →  # configure terminal  →  (config)#  →  (config-if)#
> ```
> Mode utilisateur → Privilégié → Configuration globale → Interface

---

## Configuration & Sécurité de base

> [!example] Configuration basique
> ### [[Configuration basique]]
> Premières commandes à appliquer sur tout équipement Cisco.
>
> - Nommer l'équipement : `hostname <nom>`
> - Mot de passe enable : `enable secret <mdp>`
> - Mot de passe console : `line console 0` → `password` → `login`
> - Sauvegarder : `copy running-config startup-config`

> [!example] Activer SSH
> ### [[Activer SSH]]
> Sécuriser l'accès distant en remplaçant Telnet par SSH.
>
> - Prérequis : mot de passe enable obligatoire
> - `ip domain-name <domaine>`
> - `ip ssh version 2`
> - `crypto key generate rsa` (module **1024 bits**)
> - `username admin secret <mdp>`
> - `line vty 0 15` → `transport input ssh` → `login local`

---

## Services réseau

> [!info] DHCP
> ### [[DHCP]]
> Configurer un routeur Cisco comme serveur DHCP.
>
> ```
> ip dhcp pool <NOM>
>   network <réseau> <masque>
>   dns-server <IP>
>   default-router <passerelle>
> ip dhcp excluded-address <début> <fin>
> ```
>
> - `ip dhcp pool` — nom de l'étendue
> - `network` — plage distribuée
> - `ip dhcp excluded-address` — adresses réservées

> [!info] VLANs
> ### [[VLANs]]
> Segmenter le réseau en réseaux logiques isolés.
>
> - Créer : `vlan <id>` → `name <nom>`
> - Attribuer un port : `switchport mode access` → `switchport access vlan <id>`
> - Interface SVI : `interface vlan <id>` → `ip address` → `no shutdown`
> - Trunk : `switchport mode trunk` → `switchport trunk allowed vlan <liste>`
> - Téléphonie VoIP : `switchport voice vlan <id>` + `mls qos trust cos`
> - Supprimer : `no vlan <id>` (réattribuer les ports d'abord)

---

## Référence des commandes

> [!note] Liste de commandes
> ### [[Liste de commandes]]
> Tableau de référence complet des commandes IOS les plus utilisées en TP.

| Tâche | Commande IOS |
|---|---|
| Mode privilégié | `enable` |
| Mode configuration globale | `configure terminal` |
| Nommer l'équipement | `hostname <nom>` |
| Adresse IP sur interface | `ip address <IP> <masque>` |
| Activer une interface | `no shutdown` |
| Adresse IPv6 sur interface | `ipv6 address <adresse>/<préfixe>` |
| Passerelle par défaut (switch) | `ip default-gateway <IP>` |
| Interface SVI | `interface vlan <id>` |
| Port en mode accès | `switchport mode access` |
| Port en mode trunk | `switchport mode trunk` |
| VLAN natif sur trunk | `switchport trunk native vlan <id>` |
| VLANs autorisés sur trunk | `switchport trunk allowed vlan <liste>` |
| Vérifier les VLANs | `show vlan brief` |
| Vérifier SSH | `show ip ssh` |
| Sauvegarder la config | `copy running-config startup-config` |

---

> [!tip] Ordre de configuration recommandé
> 1. Appliquer la **configuration de base** (nom, mots de passe, sauvegarde) → [[Configuration basique]]
> 2. Activer **SSH** pour sécuriser l'accès à distance → [[Activer SSH]]
> 3. Créer et attribuer les **VLANs**, configurer les trunks → [[VLANs]]
> 4. Configurer le service **DHCP** si le routeur distribue les adresses → [[DHCP]]
> 5. Vérifier avec `show vlan brief`, `show ip ssh`, `show running-config`
