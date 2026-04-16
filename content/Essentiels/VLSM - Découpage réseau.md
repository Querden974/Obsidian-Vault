---
tags:
  - réseau
  - vlsm
  - sous-réseau
  - astuces
Catégorie:
  - Réseau
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T00:00:00
---

# VLSM — Astuces de découpage réseau

> [!abstract] C'est quoi le VLSM ?
> Le **VLSM** (Variable Length Subnet Masking) permet d'attribuer des masques de **longueurs différentes** à chaque sous-réseau, en fonction du nombre d'hôtes réellement nécessaires. Contrairement au découpage fixe (FLSM), on évite le gaspillage d'adresses.
>
> **Règle d'or : toujours trier les sous-réseaux du plus grand au plus petit avant de commencer.**

---

## La table des puissances de 2 — à connaître par cœur

| Puissance | Valeur | Hôtes utilisables | Préfixe (/n) dans un /24 |
|---|---|---|---|
| 2¹ | 2 | 0 | — |
| 2² | 4 | 2 | /30 |
| 2³ | 8 | 6 | /29 |
| 2⁴ | 16 | 14 | /28 |
| 2⁵ | 32 | 30 | /27 |
| 2⁶ | 64 | 62 | /26 |
| 2⁷ | 128 | 126 | /25 |
| 2⁸ | 256 | 254 | /24 |

> [!tip] Astuce
> **Hôtes utilisables = taille du bloc − 2** (adresse réseau + broadcast retirées)
> Pour trouver le préfixe : `32 − log₂(taille du bloc)` — ou simplement mémoriser la table.

---

## Méthode en 4 étapes

> [!example] Étape 1 — Trier les besoins du plus grand au plus petit
> Liste tous les sous-réseaux demandés avec leur nombre d'hôtes requis, puis **classe-les par ordre décroissant**.
>
> Exemple pour `192.168.1.0/24` :
>
> | Sous-réseau | Hôtes requis |
> |---|---|
> | LAN A | 100 hôtes |
> | LAN B | 50 hôtes |
> | LAN C | 20 hôtes |
> | Lien WAN | 2 hôtes |

> [!example] Étape 2 — Trouver la taille de bloc pour chaque besoin
> Prendre la **première puissance de 2 supérieure ou égale** au nombre d'hôtes requis **+ 2**.
>
> | Sous-réseau | Hôtes requis | Hôtes + 2 | Bloc (2ⁿ) | Préfixe |
> |---|---|---|---|---|
> | LAN A | 100 | 102 | 128 (2⁷) | /25 |
> | LAN B | 50 | 52 | 64 (2⁶) | /26 |
> | LAN C | 20 | 22 | 32 (2⁵) | /27 |
> | Lien WAN | 2 | 4 | 4 (2²) | /30 |

> [!example] Étape 3 — Attribuer les blocs dans l'ordre
> Partir de l'adresse de départ et attribuer chaque bloc **sans chevauchement**.
>
> | Sous-réseau | Adresse réseau | Plage hôtes | Broadcast | Préfixe |
> |---|---|---|---|---|
> | LAN A | 192.168.1.0 | .1 → .126 | .127 | /25 |
> | LAN B | 192.168.1.128 | .129 → .190 | .191 | /26 |
> | LAN C | 192.168.1.192 | .193 → .222 | .223 | /27 |
> | Lien WAN | 192.168.1.224 | .225 → .226 | .227 | /30 |

> [!example] Étape 4 — Vérifier
> - Adresse réseau suivante = broadcast précédent + 1 ✔
> - Aucun bloc ne déborde hors de l'espace `192.168.1.0/24` ✔
> - Total utilisé : 128 + 64 + 32 + 4 = **228 adresses** sur 256

---

## Astuces de calcul rapide

> [!tip] Trouver l'adresse de broadcast sans calculer
> ```
> Broadcast = adresse réseau + taille du bloc − 1
> ```
> Exemple : bloc de 64 commençant à `192.168.1.128`
> → Broadcast = `192.168.1.128 + 64 − 1` = `192.168.1.191`

> [!tip] Trouver l'adresse réseau suivante
> ```
> Réseau suivant = adresse réseau actuelle + taille du bloc
> ```
> Exemple : LAN B commence à `.128`, bloc de 64
> → LAN C commence à `.128 + 64` = `.192` ✔

> [!tip] Vérifier qu'un bloc est bien aligné
> L'adresse réseau d'un bloc de taille **N** doit être **un multiple de N**.
> - Bloc /26 (64 adresses) → adresse réseau doit être multiple de 64 : 0, 64, 128, 192 ✔
> - Bloc /27 (32 adresses) → multiple de 32 : 0, 32, 64, 96, 128 … ✔
>
> ⚠ Un bloc **mal aligné** causera des chevauchements.

> [!tip] Lien point-à-point entre deux routeurs
> Toujours utiliser un **/30** (4 adresses, 2 hôtes utilisables).
> C'est le plus petit bloc valide pour relier deux équipements.

---

## Tableau masques ↔ préfixes — référence rapide

| Préfixe | Masque décimal | Taille bloc | Hôtes utiles |
|---|---|---|---|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

> [!note] Astuce mnémotechnique pour le masque décimal
> La valeur dans le dernier octet suit toujours la séquence :
> `128 — 192 — 224 — 240 — 248 — 252 — 254`
> Chaque valeur = valeur précédente + moitié de l'écart restant jusqu'à 256.

---

## Erreurs fréquentes à éviter

| Erreur | Conséquence | Règle |
|---|---|---|
| Oublier les adresses réseau et broadcast | Sous-réseau trop petit | Toujours ajouter +2 au besoin |
| Ne pas trier du plus grand au plus petit | Gaspillage ou impossibilité de caser les grands blocs | Trier avant de commencer |
| Bloc mal aligné | Chevauchement d'adresses | Vérifier que l'adresse réseau est multiple de la taille du bloc |
| Utiliser un /29 pour un lien WAN | 6 adresses gaspillées | Utiliser /30 pour les liens point-à-point |

---

> [!seealso] Notes liées
> - [[Calcul sous-réseau]] — Méthode FLSM (masque fixe), incrément, broadcast
> - [[Bases des Réseaux Informatiques]] — Rappels OSI, TCP/IP, adressage
> - [[_Procédures/Centre de documents/Cisco packet Tracer/index]] — Mise en pratique sur Packet Tracer
