---

---
Comprendre comment les commutateurs (switch) organisent le trafic réseau en isolant
les *domaines de collision* et les *domaines de diffusion*, et pourquoi ces concepts sont essentiels pour éviter la congestion.

---

## **1. Domaines de collision**

| **Termes** | **Définition** | **Exemple / Illustration** |
| --- | --- | --- |
| **Collision** | Deux paquets émis simultanément sur le même segment partagé, entraînant une perte de données. | Hub (concentrateur) : plusieurs PC connectés partagent la même bande passante. |
| **Domaine de collision** | Segment réseau où les périphériques sont en concurrence pour l’accès au support partagé. Si deux appareils transmettent à la fois, une collision se produit. | Sur un hub, chaque port est dans le même domaine. |

### **Pourquoi c’est problématique ?**

- **Hub vs Switch**
    - *Hub* : tout segment = même domaine → collisions fréquentes, congestion rapide.
    - *Switch* (mode bidirectionnel non simultané) : chaque port forme son propre domaine → collision impossible entre deux ports différents.

### **Exemple concret**

> [!note]+ ### Mauvais domaine de collision
> ![[image 8.png]]

> [!note]+ ### Bon domaine de collision
> ![[image 9.png]]

**Règle pratique** : plus les domaines de collision sont petits (un port = un domaine), meilleur sera le réseau.

---

## **2. Domaines de diffusion**

| **Termes** | **Définition** | **Exemple / Illustration** |
| --- | --- | --- |
| **Diffusion** | Trame envoyée à toutes les machines d’un même LAN sans passer par un routeur. | Un PC envoie une requête ARP, tous les périphériques du VLAN l’entendent. |
| **Domaine de diffusion** | Ensemble de commutateurs interconnectés où toute trame de diffusion est relayée sur chaque port (sauf le port d’entrée). | Trois switches reliés entre eux font partie du même domaine de diffusion. |

### **Pourquoi c’est problématique ?**

- La bande passante est consommée par les trames de diffusion, réduisant l’efficacité.
- Un grand nombre de diffusions peut entraîner un *broadcast storm* (orage de broadcast) et ralentir le réseau.

### **Segmentation avec les routeurs**

- **Routeur** : sépare les domaines de diffusion (et de collision).
    - Chaque interface du routeur appartient à un domaine différent.
    - Les trames de diffusion ne traversent pas le routeur, elles restent confinées à leur LAN.

`LAN1 ── Switch ── Routeur ── Switch ── LAN2
           |                     |
         Diffusion            Diffusion`

### **Illustration du réseau d’entreprise (exemple)**

- 3 domaines de diffusion identifiés par des cercles rouges.
- Le domaine le plus grand est celui de l’ensemble de l’entreprise → source potentielle de congestion.

---

## **3. Résumé rapide**

| **Concept** | **Ce que c’est** | **Pourquoi ça compte** |
| --- | --- | --- |
| **Domaine de collision** | Segment où les collisions peuvent se produire (hub ou port partagé). | Collision = perte, besoin d’une architecture sans partage (switch). |
| **Domaine de diffusion** | Ensemble de switches où une trame est diffusée à tous. | Diffusion = consommation de bande passante; trop grand → inefficacité. |

---

## **4. Conseils pratiques pour la conception réseau**

1. **Utiliser des commutateurs** (et pas de hubs) afin que chaque port soit son propre domaine de collision.
2. **Segmenter les VLANs** pour réduire la taille des domaines de diffusion.
3. **Placer un routeur ou un pare‑feu** entre les grands segments LAN pour couper le broadcast.
4. **Surveiller l’usage du broadcast** : outils SNMP, NetFlow, etc., pour détecter un *broadcast storm*.

---

**Astuce mnémotechnique**

_« Collision » → **C**ouplage partagé (hub)

_« Diffusion » → **D**éploiement à tous (LAN entier)

---

### **Questions de révision**

5. Qu’est-ce qu’une collision et comment le switch l’élimine‑t‑il ?
6. Quelle est la différence entre un domaine de collision et un domaine de diffusion ?
7. Pourquoi les routeurs séparent-ils les domaines de diffusion ?
8. Quels sont les risques d’avoir un domaine de diffusion trop grand ?