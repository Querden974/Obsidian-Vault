---
tags:
  - routage
  - cisco
  - réseau
  - index
base: "[[_Centre de documents.base]]"
Catégorie:
  - Réseau
Heure de la dernière modification: 2026-04-01T13:35:00
Créée par: Gautier RAYEROUX
Date de création: 2026-04-01T13:36:00
Dernière modification par: Gautier RAYEROUX
---

# Routage — Centre de Ressources

> [!abstract] Présentation
> Le **routage dynamique** permet aux routeurs d'échanger automatiquement leurs tables de routage sans configuration manuelle de chaque route. Cette section couvre les trois protocoles principaux utilisés en environnement Cisco, ainsi que les travaux pratiques associés.

---

## Protocoles de routage dynamique

> [!example] RIP
> ### [[🦽Routage - RIP]]
> Protocole **vecteur de distance** — le plus simple.
>
> - Distance administrative : **120**
> - Métrique : nombre de sauts
> - Limite : **15 sauts** max
> - Mise à jour toutes les **30s** (table complète)
> - Standard ouvert (RFC)
>
> ```
> router rip
>  version 2
>  no auto-summary
>  network <réseau>
> ```

> [!example] EIGRP
> ### [[🚗Routage - EIGRP]]
> Protocole **hybride** (vecteur de distance avancé) — Cisco.
>
> - Distance administrative : **90**
> - Métrique : bande passante + délai
> - Convergence très rapide (algorithme **DUAL**)
> - Mises à jour **incrémentales** uniquement
> - Wildcard obligatoire
>
> ```
> router eigrp <AS>
>  no auto-summary
>  network <réseau> <wildcard>
> ```

> [!example] OSPF
> ### [[🏍Routage - OSPF]]
> Protocole **état de liens** — standard ouvert.
>
> - Distance administrative : **110**
> - Métrique : coût (bande passante)
> - Convergence rapide (LSA)
> - Support des **areas** (Area 0 obligatoire)
> - Wildcard + area obligatoires
>
> ```
> router ospf <ID>
>  network <réseau> <wildcard> area 0
> ```

---

## Comparaison & Synthèse

> [!info] Tableau comparatif des protocoles
> ### [[Synthèse - Protocoles de Routage Dynamique]]
> Vue comparative complète des trois protocoles pour choisir le bon selon le contexte.

| Critère | RIPv2 | EIGRP | OSPF |
|---|---|---|---|
| Distance Administrative | 120 | 90 | 110 |
| Métrique | Sauts | BP + Délai | Coût |
| Limite | 15 sauts | Illimitée | Illimitée |
| Convergence | Lente | Très rapide | Rapide |
| Standard | Ouvert | Cisco (partiel) | Ouvert |
| Lettre dans `show ip route` | `R` | `D` | `O` |

---

## Pratique & Configuration

> [!note] Configurations de référence
> ### [[Configurations Routeurs - Cisco PT]]
> Configurations complètes à appliquer sur les routeurs dans Cisco Packet Tracer.
>
> - Assignation des adresses IP aux interfaces
> - Activation des protocoles par routeur
> - Commandes `enable` / `config t`

> [!note] Travaux Pratiques
> ### [[TP 3 Routage dynamique]]
> TP combinant RIP, OSPF et EIGRP sur une topologie multi-zones.
>
> 1. Mise en place RIP + OSPF + EIGRP
> 2. Redistribution de routes entre protocoles
> 3. Tests de connectivité inter-zones

---

## Commandes utiles

| Commande | Description |
|---|---|
| `show ip route` | Afficher la table de routage |
| `show ip protocols` | Voir les protocoles actifs |
| `show ip ospf neighbor` | Vérifier les voisins OSPF |
| `show ip eigrp neighbors` | Vérifier les voisins EIGRP |
| `show ip rip database` | Table RIP |
| `debug ip rip` | Déboguer RIP en temps réel |
| `no auto-summary` | Désactiver le résumé automatique (EIGRP/RIP) |

---

> [!tip] Ordre de préférence des routes (DA)
> Quand plusieurs protocoles coexistent, le routeur choisit la route avec la **distance administrative la plus faible** :
>
> `Connecté (0)` → `Statique (1)` → `EIGRP (90)` → `OSPF (110)` → `RIP (120)`
