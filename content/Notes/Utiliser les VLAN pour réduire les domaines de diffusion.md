---

---
Les **VLAN**  assurent la **segmentation** et favorisent la flexibilité de l’entreprise. Les VLAN (**VitualLAN**) reposent sur des **connexions logiques** et non sur des connexions physiques.

> [!tip] 💡
> Les VLAN permettent à un administrateur de segmenter les réseaux en fonction de facteurs tels que la fonction, l’équipe de projet ou l’application.

Chaque VLAN constitue un **réseau logique distinct**. Les appareils d'un même VLAN fonctionnent comme sur un réseau indépendant, tout en partageant l'infrastructure avec d'autres VLAN. Tout port du commutateur peut être assigné à un VLAN.

Les VLAN optimisent les **performances réseau** en réduisant les domaines de diffusion. Une trame Ethernet diffusée par un appareil n'est reçue que par les autres appareils du même VLAN.

> [!tip] 💡
> Grâce aux VLAN, les administrateurs de réseau peuvent mettre en œuvre des politiques d’accès et de sécurité en fonction de groupes d’utilisateurs spécifiques. Chaque port de commutateur peut être attribué à un seul VLAN (à l’exception des ports connectés à un téléphone IP ou à un autre commutateur).

Chaque VLAN correspond à un réseau IP distinct, nécessitant une conception qui intègre un modèle d'adressage hiérarchique.

**L'adressage hiérarchique** applique des numéros IP aux segments réseau ou VLAN en considérant le réseau global. Des blocs d'adresses contiguës sont assignés aux périphériques d'une zone réseau spécifique.

> [!note]+ ## Exemple : Plan d’adressage **un peu simplifié**
> | **Groupes** | **VLAN ID** | **Adresse réseau** | **Première adresse disponible** | **Dernière adresse disponible** | **Passerelle réseau** |
> | --- | --- | --- | --- | --- | --- |
> | Direction | 20 | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.253 | 192.168.20.254 |
> | Examen<br>Concours | 21 | 192.168.21.0/24 | 192.168.21.1 | 192.168.21.253 | 192.168.21.254 |
> | Paie/DRH | 22 | 192.168.22.0/24 | 192.168.22.1 | 192.168.22.253 | 192.168.22.254 |
> | Emploi | 23 | 192.168.23.0/24 | 192.168.23.1 | 192.168.23.253 | 192.168.23.254 |
> | Médecine | 24 | 192.168.24.0/24 | 192.168.24.1 | 192.168.24.253 | 192.168.24.254 |
> | Assurance | 25 | 192.168.25.0/24 | 192.168.25.1 | 192.168.25.253 | 192.168.25.254 |
> | Info/RGPD | 27 | 192.168.27.0/24 | 192.168.27.1 | 192.168.27.253 | 192.168.27.254 |
> | Serveurs | 30 | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.253 | 192.168.30.254 |
> | Impression | 40 | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.253 | 192.168.40.254 |
> | Téléphones | 50 | 192.168.50.0/24 | 192.168.50.1 | 192.168.50.253 | 192.168.50.254 |
> | WIFI | 60 | 192.168.60.0/24 | 192.168.60.1 | 192.168.60.253 | 192.168.60.254 |
> | Administration | 100 | 192.168.100.0/24 | 192.168.100.1 | 192.168.100.253 | 192.168.100.254 |
> 

| **Bénéfice** | **Description** |
| --- | --- |
| Domaines de diffusion plus petits | La division d’un réseau en VLAN réduit le nombre de périphériques dans le domaine de diffusion. |
| Sécurité optimisée | Seuls les utilisateurs du même VLAN peuvent communiquer ensemble. |
| Amélioration de l’efficacité des ressources IT | Les VLAN simplifient la gestion du réseau car les utilisateurs ayant des besoins similaires peuvent être configurés sur le même VLAN, et les VLAN peuvent être nommés pour les rendre plus faciles à identifier. |
| Coût réduit | Les VLAN réduisent la nécessité de mises à niveau coûteuses du réseau et utilisent plus efficacement la largeur de bande et les liaisons montantes existantes, ce qui permet de réaliser des économies. |
| Meilleures performances | Les domaines de diffusion plus petits réduisent le trafic inutile sur le réseau, et améliorent les performances. |
| Une gestion simplifiée des projets et des applications | Les VLAN regroupent les utilisateurs et les périphériques réseau pour prendre en charge l’entreprise ou les exigences géographiques ; cela permet d’avoir des fonctions distinctes qui rendent la gestion d’un projet ou l’utilisation d’une application spécialisée plus facile. |

# **Identifiez les types de VLAN**

### **1. VLAN par défaut**

Le VLAN par défaut sur un commutateur Cisco est le VLAN 1. Par conséquent, tous les ports du commutateur sont sur le VLAN 1. Ce qu’il faut savoir :

- Tous les ports sont attribués à VLAN 1 par défaut.
- Le VLAN natif est le VLAN 1 par défaut.
- Le VLAN de gestion est le VLAN 1 par défaut.
- Le VLAN 1 ne peut être renommé ni supprimé.

La commande `show vlan brief` permet de connaître l’appartenance des VLAN en fonction des interfaces. Tous les ports sont actuellement attribués au VLAN 1 par défaut. Aucun VLAN natif n’est explicitement attribué et aucun autre VLAN n’est actif ; par conséquent, le VLAN natif est défini comme VLAN de gestion. Il s’agit d’un risque de sécurité.

### **2. VLAN de données**

Les VLAN de données sont des VLAN configurés pour séparer le trafic généré par l’utilisateur. Les VLAN de données sont utilisés pour diviser un réseau en groupes d’utilisateurs ou de périphériques.

### **3. VLAN natif**

Le trafic utilisateur à partir d’un VLAN doit être marqué avec son **ID VLAN** lorsqu’il est envoyé à un autre commutateur. Les ports de **Trunk** sont utilisés entre les commutateurs pour prendre en charge la transmission du **trafic balisé**.

Le VLAN natif sur un commutateur Cisco est le VLAN 1 (VLAN par défaut). Il est généralement recommandé de configurer le VLAN natif en tant que VLAN inutilisé, distinct du VLAN 1 et des autres VLAN. En fait, il n’est pas rare de dédier un VLAN fixe jouant le rôle de VLAN natif pour tous les ports Trunk du domaine commuté.

### **3. VLAN de gestion**

Un VLAN de gestion est un VLAN de données configuré spécifiquement pour le trafic de gestion réseau, y compris SSH, Telnet, HTTPS, HTTP et SNMP. Par défaut, le VLAN 1 est configuré comme VLAN de gestion sur un commutateur de couche 2.

### **4. VLAN voix**

Un VLAN distinct est nécessaire pour prendre en charge la voix sur IP (VoIP). Le trafic VoIP requiert les éléments suivants :

- Bande passante consolidée pour garantir la qualité de la voix.
- Priorité de transmission par rapport aux autres types de trafic réseau.
- Possibilité de routage autour des zones encombrées du réseau.
- Délai (ping) inférieur à 150 ms sur tout le réseau.