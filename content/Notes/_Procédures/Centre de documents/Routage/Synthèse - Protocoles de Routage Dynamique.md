# Synthèse — Protocoles de Routage Dynamique

> Fichier de synthèse basé sur : [[🦽Routage - RIP]], [[🚗Routage - EIGRP]], [[🏍Routage - OSPF]]

---

## 1. Tableau comparatif

| Critère | RIPv2 | EIGRP | OSPF |
|---------|-------|-------|------|
| **Type** | Vecteur de distance | Vecteur de distance avancé (hybride) | État de liens (Link-State) |
| **Distance Administrative** | 120 | 90 | 110 |
| **Métrique** | Nombre de sauts | Bande passante + Délai | Coût (basé sur la bande passante) |
| **Limite** | 15 sauts max | Illimité (théorique) | Illimité |
| **Convergence** | Lente (30s entre MàJ) | Très rapide (DUAL) | Rapide (LSA) |
| **Mise à jour** | Toutes les 30s, table complète | Incrémentale (uniquement les changements) | Incrémentale (LSA sur changement) |
| **Support VLSM / CIDR** | Oui (v2) | Oui | Oui |
| **Propriétaire** | Standard ouvert (RFC) | Propriétaire Cisco (partiellement ouvert) | Standard ouvert (RFC 2328) |
| **Areas / Zones** | Non | Non | Oui (Area 0 obligatoire) |
| **Commande de base** | `router rip` | `router eigrp <AS>` | `router ospf <ID>` |
| **Wildcard requis** | Non | Oui | Oui |

---

## 2. Résumé de chaque protocole

### RIPv2 — Routing Information Protocol v2

- Protocole **le plus simple** à configurer.
- Métrique = **nombre de sauts** uniquement (pas de prise en compte de la bande passante).
- Limité à **15 sauts** : réseau injoignable à partir de 16 routeurs.
- Envoie la **table de routage complète** toutes les **30 secondes** à tous les voisins.
- Temporisations clés :
  - **30s** — mise à jour de la table
  - **180s** — route marquée injoignable
  - **240s** — route supprimée de la table
- `no auto-summary` et `no auto-summary` recommandés pour éviter les agrégations classful.

```cisco
router rip
 version 2
 no auto-summary
 network 10.0.0.0
```

---

### EIGRP — Enhanced Interior Gateway Routing Protocol

- Protocole **hybride** (combine vecteur de distance et état de liens).
- Métrique basée sur la **bande passante** et le **délai** (plus intelligent que RIP).
- Distance administrative de **90** → préféré sur RIP (120) et OSPF (110) si les trois coexistent.
- Convergence **très rapide** grâce à l'algorithme **DUAL** (Diffusing Update Algorithm).
- N'envoie que les **mises à jour partielles** (uniquement les changements).
- **Propriétaire Cisco** à l'origine (partiellement ouvert depuis 2013).
- Nécessite un **numéro de système autonome** (AS) identique sur tous les routeurs du même domaine.
- Bonnes pratiques à appliquer systématiquement :
  - `no auto-summary` — évite les routes `Null0` dangereuses
  - `passive-interface` — bloque les Hello sur les interfaces terminales
  - `eigrp router-id` — fixe un ID stable

```cisco
router eigrp 1
 no auto-summary
 eigrp router-id 1.1.1.1
 passive-interface FastEthernet0/0
 network 10.0.0.0 0.0.0.3
```

---

### OSPF — Open Shortest Path First

- Protocole **état de liens**, standard ouvert (multi-constructeur).
- Métrique = **coût** calculé en fonction de la bande passante du lien.
- Convergence **rapide** par échange de LSA (Link-State Advertisements).
- Supporte la notion d'**areas** (zones) pour segmenter et optimiser les grands réseaux.
  - L'**Area 0** (backbone) est obligatoire — toutes les autres areas doivent y être reliées.
- Requiert un **wildcard** et une **area** dans la commande `network`.
- Supporte le **VLSM** et le **CIDR** nativement.

```cisco
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
```

---

## 3. Redistribution entre protocoles

Quand plusieurs protocoles coexistent, il faut redistribuer les routes d'un protocole vers l'autre.

### Depuis RIP

```cisco
router rip
 redistribute static
 redistribute ospf 1 metric 5
```

### Depuis EIGRP

```cisco
router eigrp 1
 redistribute static
 redistribute rip metric 10000 10 255 1 1500
 redistribute ospf 1 metric 10000 100 255 1 1500
```

> Les 5 valeurs EIGRP : **bande passante (Kb/s)** — **délai (x10µs)** — **fiabilité (0-255)** — **charge (1-255)** — **MTU (octets)**  
> En pratique, seules la bande passante et le délai sont utilisées par EIGRP.

### Depuis OSPF

```cisco
router ospf 1
 redistribute static
 redistribute rip subnets
```

---

## 4. Quand utiliser quel protocole ?

### Utiliser RIPv2 si...

- Réseau **très petit** (< 10 routeurs, clairement < 15 sauts).
- Environnement de **lab / maquette** ou **apprentissage**.
- Pas besoin de performances ou de convergence rapide.
- Infrastructure **multi-constructeur** sans support OSPF.

> **Ne pas utiliser RIP en production** sur un réseau réel de taille moyenne ou grande.  
> La convergence lente (30s) et la limite des 15 sauts le rendent inadapté.

---

### Utiliser EIGRP si...

- Infrastructure **100% Cisco** (ou compatible Cisco).
- Réseau de **taille moyenne à grande** avec besoin de convergence rapide.
- On veut une configuration **plus simple qu'OSPF** mais plus performante que RIP.
- On a besoin de **load balancing** sur des liens de coût inégal (inégal-cost load balancing).
- On veut éviter la complexité des areas OSPF.

> EIGRP est souvent le choix par défaut dans les réseaux **d'entreprise full Cisco**.  
> Sa distance administrative de 90 le rend prioritaire sur OSPF et RIP.

---

### Utiliser OSPF si...

- Infrastructure **multi-constructeur** (Cisco + HP + Juniper, etc.).
- Réseau **large ou très large** (WAN, multi-sites, opérateurs).
- On a besoin de **segmentation en areas** pour optimiser le trafic et les tables de routage.
- On doit respecter des **standards ouverts** (appels d'offre, interopérabilité).
- Environnement **ISP / opérateur**.

> OSPF est le protocole IGP de référence en entreprise et chez les opérateurs.  
> C'est le plus **scalable** et le plus **universel** des trois.

---

## 5. Arbre de décision

```
Réseau de lab / apprentissage ?
    └── OUI → RIPv2

Infrastructure 100% Cisco ?
    └── OUI → EIGRP (sauf si multi-constructeur requis)
    └── NON → OSPF

Grand réseau multi-sites ou multi-constructeur ?
    └── OUI → OSPF

Besoin de load balancing sur liens inégaux ?
    └── OUI → EIGRP

Standard ouvert obligatoire ?
    └── OUI → OSPF
```

---

## 6. Récapitulatif des distances administratives

> La **distance administrative (DA)** détermine quel protocole est préféré quand plusieurs proposent une route vers la même destination. **Plus la DA est basse, plus la route est prioritaire.**

| Source de la route | DA |
|--------------------|----|
| Directement connectée | 0 |
| Route statique | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

> Si EIGRP et OSPF proposent tous les deux une route vers `192.168.1.0/24`, le routeur utilisera celle d'**EIGRP** (DA 90 < 110).
