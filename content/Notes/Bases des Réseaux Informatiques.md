# Bases des Réseaux Informatiques

## 1. Définitions fondamentales

- **Réseau informatique** : ensemble d'équipements (ordinateurs, serveurs, imprimantes, etc.) interconnectés pour partager des ressources et des données.
- **Protocole** : ensemble de règles définissant la communication entre équipements.
- **Topologie** : organisation physique ou logique du réseau.

---

## 2. Types de réseaux

| Acronyme | Nom | Portée |
|----------|-----|--------|
| PAN | Personal Area Network | ~10 m (Bluetooth, USB) |
| LAN | Local Area Network | Bâtiment / site |
| MAN | Metropolitan Area Network | Ville / agglomération |
| WAN | Wide Area Network | Pays / continent / monde |
| VPN | Virtual Private Network | Réseau privé sur réseau public |

---

## 3. Topologies réseau

| Topologie | Description | Avantages | Inconvénients |
|-----------|-------------|-----------|---------------|
| Bus | Tous reliés à un câble commun | Simple, peu de câble | Panne du bus = tout arrêté |
| Étoile | Tous reliés à un équipement central (switch) | Panne isolée, facile à gérer | Dépend du switch central |
| Anneau | Chaque équipement relié au suivant | Débit stable | Panne d'un nœud = rupture |
| Maillée | Chaque équipement relié à plusieurs autres | Haute disponibilité | Coût et complexité élevés |

---

## 4. Équipements réseau

| Équipement | Couche OSI | Rôle |
|------------|------------|------|
| Répéteur / Hub | Couche 1 | Régénère le signal, diffusion à tous |
| Switch (commutateur) | Couche 2 | Commutation par adresse MAC |
| Routeur | Couche 3 | Routage par adresse IP entre réseaux |
| Pare-feu (Firewall) | Couche 3-7 | Filtrage du trafic réseau |
| Point d'accès Wi-Fi | Couche 2 | Connexion sans fil |
| Modem | Couche 1 | Modulation/démodulation du signal |

---

## 5. Le modèle OSI

> **OSI** = Open Systems Interconnection, standardisé par l'ISO en 1984.  
> Modèle de référence en **7 couches** décrivant comment les données transitent d'un équipement à un autre.

### Schéma des 7 couches

```
+---+-----------------------------+----------------------------------+
| 7 | Application                 | Interface avec l'utilisateur     |
+---+-----------------------------+----------------------------------+
| 6 | Présentation                | Format, chiffrement, compression |
+---+-----------------------------+----------------------------------+
| 5 | Session                     | Gestion des sessions             |
+---+-----------------------------+----------------------------------+
| 4 | Transport                   | Segmentation, fiabilité (TCP/UDP)|
+---+-----------------------------+----------------------------------+
| 3 | Réseau                      | Adressage IP, routage            |
+---+-----------------------------+----------------------------------+
| 2 | Liaison de données          | Adresse MAC, accès au support    |
+---+-----------------------------+----------------------------------+
| 1 | Physique                    | Signal électrique / optique      |
+---+-----------------------------+----------------------------------+
```

### Détail de chaque couche

#### Couche 1 — Physique
- Transmet les bits sous forme de signaux (électriques, optiques, radio).
- Définit les connecteurs, câbles, débits, encodages.
- Exemples : RJ45, fibre optique, Wi-Fi (signal), Ethernet physique.

#### Couche 2 — Liaison de données
- Structuration des données en **trames**.
- Adressage par **adresse MAC** (48 bits, ex: `AA:BB:CC:DD:EE:FF`).
- Détection d'erreurs (CRC).
- Sous-couches : **LLC** (contrôle logique) et **MAC** (accès au support).
- Exemples : Ethernet, Wi-Fi (802.11), PPP.

#### Couche 3 — Réseau
- Adressage logique : **adresse IP** (IPv4 / IPv6).
- **Routage** des paquets entre réseaux différents.
- Fragmentation des paquets si nécessaire.
- Exemples : IP, ICMP, ARP, OSPF, BGP.

#### Couche 4 — Transport
- **Segmentation** des données et réassemblage.
- Contrôle de flux et de congestion.
- Deux protocoles principaux :
  - **TCP** (Transmission Control Protocol) : fiable, orienté connexion (3-way handshake).
  - **UDP** (User Datagram Protocol) : rapide, non fiable, sans connexion.
- Notion de **port** (ex: HTTP = 80, HTTPS = 443, SSH = 22).

#### Couche 5 — Session
- Établissement, gestion et fermeture des **sessions** de communication.
- Synchronisation et reprise en cas d'interruption.
- Exemples : NetBIOS, RPC, SIP.

#### Couche 6 — Présentation
- **Formatage** des données pour qu'elles soient lisibles par l'application.
- **Chiffrement / déchiffrement** (SSL/TLS agit ici).
- **Compression** des données.
- Exemples : SSL/TLS, JPEG, MPEG, ASCII, UTF-8.

#### Couche 7 — Application
- Interface directe avec **l'utilisateur et les applications**.
- Fournit les services réseau aux programmes.
- Exemples : HTTP, HTTPS, FTP, DNS, SMTP, POP3, IMAP, SNMP, Telnet, SSH.

---

### Mnémotechnique OSI (de la couche 7 à 1)

> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing  
> Application — Présentation — Session — Transport — Réseau (Network) — Liaison (Data link) — Physique (Processing)

---

## 6. Encapsulation / Décapsulation

Quand des données descendent les couches OSI côté émetteur, chaque couche **encapsule** les données en ajoutant un en-tête (et parfois un pied).

```
[Application]  Données
[Transport]    [En-tête TCP/UDP | Données]          → Segment
[Réseau]       [En-tête IP | Segment]                → Paquet
[Liaison]      [En-tête Ethernet | Paquet | FCS]     → Trame
[Physique]     101010011011...                        → Bits
```

Le récepteur effectue la **décapsulation** en remontant les couches.

---

## 7. Modèle TCP/IP (vs OSI)

Le modèle TCP/IP est le modèle pratique utilisé sur Internet. Il simplifie les 7 couches OSI en **4 couches**.

| Modèle TCP/IP | Couches OSI équivalentes |
|---------------|--------------------------|
| Application | 7 — Application, 6 — Présentation, 5 — Session |
| Transport | 4 — Transport |
| Internet | 3 — Réseau |
| Accès réseau | 2 — Liaison, 1 — Physique |

---

## 8. Adressage IP

### IPv4
- Adresse sur **32 bits**, notée en 4 octets décimaux : `192.168.1.10`
- Composée de deux parties : **réseau** + **hôte** (définie par le masque)
- Classes d'adresses :

| Classe | Plage | Masque par défaut |
|--------|-------|-------------------|
| A | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 (/8) |
| B | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 (/16) |
| C | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 (/24) |

- Adresses privées (RFC 1918) :
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`

### IPv6
- Adresse sur **128 bits**, notée en hexadécimal : `2001:0db8:85a3::8a2e:0370:7334`
- Élimine le besoin du NAT grâce à l'espace d'adressage massif.

---

## 9. Protocoles essentiels

| Protocole | Port | Couche | Rôle |
|-----------|------|--------|------|
| HTTP | 80 | Application | Navigation web |
| HTTPS | 443 | Application | Navigation web chiffrée |
| FTP | 20/21 | Application | Transfert de fichiers |
| SSH | 22 | Application | Accès distant sécurisé |
| Telnet | 23 | Application | Accès distant non sécurisé |
| SMTP | 25 | Application | Envoi d'e-mails |
| DNS | 53 | Application | Résolution de noms |
| DHCP | 67/68 | Application | Attribution automatique d'IP |
| SNMP | 161 | Application | Supervision réseau |
| ICMP | — | Réseau | Diagnostic (ping, traceroute) |
| ARP | — | Liaison | Résolution IP → MAC |

---

## 10. DNS — Domain Name System

- Traduit un **nom de domaine** en **adresse IP** (et inversement).
- Fonctionnement en arbre hiérarchique :
  ```
  . (racine)
  └── com
      └── google
          └── www  →  142.250.74.68
  ```
- Types d'enregistrements :
  - **A** : nom → IPv4
  - **AAAA** : nom → IPv6
  - **MX** : serveur de messagerie
  - **CNAME** : alias
  - **PTR** : IP → nom (résolution inverse)

---

## 11. DHCP — Dynamic Host Configuration Protocol

- Attribue automatiquement à un client :
  - Adresse IP
  - Masque de sous-réseau
  - Passerelle par défaut
  - Serveur DNS

- Processus **DORA** :
  1. **Discover** — le client diffuse une demande
  2. **Offer** — le serveur propose une IP
  3. **Request** — le client accepte l'offre
  4. **Acknowledge** — le serveur confirme l'attribution

---

## 12. NAT — Network Address Translation

- Permet à plusieurs hôtes d'un réseau privé de partager **une seule adresse IP publique**.
- Le routeur traduit les adresses privées ↔ adresse publique.
- Types :
  - **NAT statique** : 1 IP privée ↔ 1 IP publique fixe
  - **NAT dynamique** : pool d'IPs publiques
  - **PAT / NAT overload** : plusieurs IPs privées → 1 IP publique (via numéros de ports)

---

## 13. VLAN — Virtual Local Area Network

- Permet de **segmenter logiquement** un réseau physique en plusieurs réseaux isolés.
- Configuration sur les **switches manageable**.
- Avantages : sécurité, réduction du broadcast, organisation.
- Marquage des trames : standard **802.1Q** (tag VLAN dans l'en-tête Ethernet).
- **Trunk** : lien transportant plusieurs VLANs entre switches.
- **Access** : port affecté à un seul VLAN (vers un hôte final).
