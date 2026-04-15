---

---
### 1️⃣ **Préparation & Planification**

1. **Définir le réseau**
    - Nombre de clients (ex : 5-10)
    - Services à implémenter sur le serveur (DHCP, DNS, HTTP, FTP, Mail…)
    - Plage d’adresses IP
    - Masque de sous-réseau
2. **Dessiner un schéma réseau** (papier ou logiciel)
    - Routeur ↔ Switch ↔ Clients
    - Serveur relié au switch
3. **Lister le matériel dans Packet Tracer**
    - 1 Routeur (type 2811 ou équivalent)
    - 1 Switch (type 2960)
    - 1 Serveur
    - Plusieurs PC clients
    - Câbles (Copper Straight pour PC/Switch, Copper Cross-over pour Switch/Routeur si nécessaire)

---

### 2️⃣ **Implémentation physique dans Packet Tracer**

4. Placer tous les équipements dans la zone de travail
5. Connecter avec les bons câbles :
    - PC/Serveur ↔ Switch : **Copper Straight**
    - Switch ↔ Routeur : **Copper Straight**
6. Allumer les équipements (bouton On/Off)

---

### 3️⃣ **Configuration du routeur**

7. **Configurer les interfaces**
    - Exemple : `FastEthernet0/0` : IP 192.168.1.1 / 255.255.255.0
    - Activer l’interface (`no shutdown`)
8. **Configurer un nom d’hôte et mot de passe**
```plain text
Router> enable
Router# configure terminal
Router(config)# hostname R1
Router(config)# enable secret cisco123
Router(config)# line console 0
Router(config-line)# password admin
Router(config-line)# login
Router(config-line)# exit
```
9. **Activer le routage interne** (si plusieurs réseaux/subnets sont prévus)
    - Exemple : `ip routing`

---

### 4️⃣ **Configuration du serveur**

10. **Adresse IP statique**
    - IP : 192.168.1.2 / 255.255.255.0
    - Gateway : 192.168.1.1 (Routeur)
11. **Activation des services**
    - DHCP (optionnel si le serveur gère l’adressage)
        - Plage : 192.168.1.100 → 192.168.1.150
        - Masque : 255.255.255.0
        - Gateway : 192.168.1.1
    - DNS
        - Associer un nom de domaine interne (ex : `intranet.local`) à l’IP du serveur
    - HTTP / HTTPS
        - Mettre un fichier HTML simple pour test
    - FTP
        - Créer un compte test
12. **Tester le serveur en local**
    - Depuis l’onglet “Desktop” → Command Prompt → `ping 192.168.1.1`
    - `ping` vers les clients (une fois configurés)

---

### 5️⃣ **Configuration des clients**

13. **DHCP ou IP statique**
    - Si DHCP actif → mode “DHCP” dans l’onglet “IP Configuration”
    - Sinon → IP statique + Gateway = 192.168.1.1 + DNS = 192.168.1.2
14. **Test de connectivité**
    - `ping` vers le serveur
    - `ping` vers le routeur
15. **Test des services**
    - Ouvrir un navigateur (Desktop → Web Browser) → entrer l’IP ou domaine DNS du serveur

---

### 6️⃣ **Vérifications & Dépannage**

16. **Test réseau global**
    - `ping` et `tracert` entre toutes les machines
    - Vérifier la table ARP (`arp -a`)
17. **Test DHCP**
    - Vérifier que les adresses sont bien attribuées automatiquement
18. **Test DNS**
    - `ping intranet.local` → doit résoudre vers IP du serveur
19. **Test Web**
    - Naviguer vers `http://intranet.local`
20. **Test FTP**
    - Ouvrir le client FTP dans Packet Tracer et se connecter
21. **Contrôler la configuration**
    - Sauvegarder la config du routeur et serveur (`copy running-config startup-config`)

---

### 7️⃣ **Documentation**

22. **Schéma final du réseau**
23. **Tableau des IP**
24. **Liste des services activés**
25. **Captures d’écran des tests réussis**
26. **Fichier .pkt sauvegardé et commenté**


> [!note]+ ## Configurer SSH sur commutateur (Switch)
> Pour configurer la connexion **SSH** sur un commutateur, il faut avoir bien respecté les étapes précédentes : à savoir l’attribution d’un **nom d’hôte** et l’attribution d’une **adresse IP**. Si c’est le cas, vous pouvez procéder comme ceci :
> 
> ```plain text
> Dir-Exam#show ip ssh
> Dir-Exam#configure terminal
> Dir-Exam(config)#enable secret 1234-MetroPole:1234
> Dir-Exam(config)#ip domain-name metropolecg.com
> Dir-Exam(config)#ip ssh version 2
> Dir-Exam(config)#crypto key generate rsa
> How many bits in tje modulus [512]:1024
> Dir-Exam(config)#username admin secret 1234-MetroPole:1234
> Dir-Exam(config)#line vty 0 15
> Dir-Exam(config-line)#transport input ssh
> Dir-Exam(config-line)#login local
> Dir-Exam(config-line)#exit
> ```
> 
> > [!tip] 💡
> > **VTY** correspond aux **interfaces virtuelles** pour l’accès à distance. Le fait de limiter les connexions sur les lignes `vty`  de 0 à 15 permet d’**empêcher** les connexions **non SSH** (telles que Telnet), et **limite** le commutateur à n’accepter que les **connexions SSH**.

> [!note]+ ## Configuration VLAN sur un commutateur (Switch)
> Pour configurer le commutateur Cisco Catalyst, il est important de respecter les tâches suivantes :
> 
> ```plain text
> Switch(config)#hostname Dir-Exam
> Dir-Exam(config)#interface vlan 100
> Dir-Exam(config-if)#ip address 192.168.100.2 255.255.255.0
> Dir-Exam(config)# ip default-gateway 192.168.100.254
> Dir-Exam(config-if)#ipv6 address 2001:db8:acad:100::2/64
> Dir-Exam(config-if)#no shutdown
> ```
> 
> > [!info] ℹ️
> > Le SVI pour le VLAN 100 n'apparaîtra pas comme “up/up” jusqu’à ce que le VLAN 100 soit **créé**, et qu’un appareil soit **connecté** à un port de commutation associé au VLAN 100.
> 
> > [!warning] ⚠️
> > La **configuration IPv6** des commutateurs 2960 dans Cisco Packet Tracer **n’est pas activée**. Il vous faut entrer la commande`**sdm prefer dual-ipv4-and-ipv6 default**`dans le mode de configuration global, et retourner dans le mode de configuration privilégié pour faire un`**reload**`. Cette commande activera la fonctionnalité **IPv6** de tous les commutateurs présents dans le schéma réseau sous Cisco Packet Tracer.

> [!note]+ ## Commandes CLI
> | Tâches | Commandes IOS |
> | --- | --- |
> | Passer en mode d'exécution privilégié | Switch> enable |
> | Passer en mode de configuration globale | Switch# configure terminal |
> | Changer le nom d'hôte du commutateur | Switch(config)# hostname Dir-Exam |
> | Passer en mode de configuration d'interface pour SVI | Dir-Exam(config)# interface vlan 100 |
> | Configurer l'adresse IPv4 de l'interface de gestion | Dir-Exam(config-if)# ip address 192.168.100.2 255.255.255.0 |
> | Configurer l'adresse IPv6 de l'interface de gestion | Dir-Exam(config-if)# ipv6 address 2001:db8:acad:100::2/64 |
> | Activer l'interface de gestion | Dir-Exam(config-if)# no shutdown |
> | Repasser en mode d'exécution privilégié | Dir-Exam(config-if)# end |
> | Enregistrer la configuration en cours dans la configuration de démarrage | Dir-Exam# copy running-config startup-config |
> | Configurer la passerelle par défaut pour le commutateur | Dir-Exam(config)# ip default-gateway 192.168.100.254 |
> | Vérifier le support de SSH | Dir-Exam# show ip ssh |
> | Configurer un mot de passe pour l'accès au mode d'exécution privilégié | Dir-Exam(config)# enable secret 1234-MetroPole:1234 |
> | Configurer un nom de domaine | Dir-Exam(config)# ip domain-name [metropolecg.com](http://metropolecg.com/) |
> | Configurer la version 2 du protocole SSH | Dir-Exam(config)# ip ssh version 2 |
> | Générer des paires de clés RSA et choisir une taille de module de 1024 bits | Dir-Exam(config)# crypto key generate rsa |
> | How many bits in the modulus [512]: | 1024 |
> | Définir un nom d'utilisateur et un mot de passe | Dir-Exam(config)# username admin secret 1234-MetroPole:1234 |
> | Configurer les lignes vty | Dir-Exam(config)# line vty 0 15<br>Dir-Exam(config-line)# transport input ssh<br>Dir-Exam(config-line)# login local<br>Dir-Exam(config-line)# exit |
> | Passer en mode de configuration globale | Dir-Exam# configure terminal |
> | Créer un VLAN avec un numéro d'identité valide | Dir-Exam(config)# vlan 20 |
> | Indiquer un nom unique pour identifier le VLAN | Dir-Exam(config-vlan)# name Direction |
> | Repasser en mode d'exécution privilégié | Dir-Exam(config-vlan)# end |
> | Aller dans l'interface | Dir-Exam(config)# int fa0/1 |
> | Configurer le mode d'accès du VLAN | Dir-Exam(config-if)# switchport mode access |
> | Attribuer le VLAN à l'interface | Dir-Exam(config-if)# switchport access vlan 20 |
> | Repasser en mode d'exécution privilégié | Dir-Exam(config-if)# end |
> | Passer en mode de configuration d'interface | Dir-Exam(config)# interface g0/1 |
> | Régler le port en mode de trunking permanent | Dir-Exam(config-if)# switchport mode trunk |
> | Choisir un VLAN natif autre que le VLAN 1 | Dir-Exam(config-if)# switchport trunk native vlan 100 |
> | Indiquer la liste des VLANs autorisés sur la liaison Trunk | Dir-Exam(config-if)# switchport trunk allowed vlan 20,21,40,50,100 |
> | Repasser en mode d'exécution privilégié | Dir-Exam(config-vlan)# end |

> [!note]+ ## Configuration type commutateur: 
> ```plain text
> Router> enable
> Router# configure terminal
> Enter configuration commands, one per line.  End with 
> CNTL/Z.
> Router(config)# hostname RouteurVPN
> RouteurVPN(config)# enable secret 1234-MetroPole:1234
> RouteurVPN(config)# ip ssh version 2
> RouteurVPN(config)# ip domain-name metropolecg.com
> RouteurVPN(config)# username admin secret 
> 1234-MetroPole:1234
> RouteurVPN(config)# crypto key generate rsa
> The name for the keys will be: RouteurVPN.metropolecg.com
> Choose the size of the key modulus in the range of 360 to 
> 2048 for your
>  General Purpose Keys. Choosing a key modulus greater than 
> 512 may take
>  a few minutes.
> 
> How many bits in the modulus [512]: 1024
> % Generating 1024 bit RSA keys, keys will be 
> non-exportable...[OK]
> 
> RouteurVPN(config)# line console 0
> RouteurVPN(config-line)# password 1234-MetroPole:1234
> RouteurVPN(config-line)# login
> RouteurVPN(config-line)# exit
> RouteurVPN(config)# line vty 0 15
> RouteurVPN(config-line)# transport input ssh
> RouteurVPN(config-line)# login local
> RouteurVPN(config-line)# exit
> RouteurVPN(config)# service password-encryption
> RouteurVPN(config)# banner motd #Acces aux Personnes 
> Autorisees Seulement !#
> RouteurVPN(config)# exit
> RouteurVPN#
> %SYS-5-CONFIG_I: Configured from console by console
> 
> RouteurVPN# copy running-config startup-config
> Destination filename [startup-config]?
> Building configuration...
> [OK]
> RouteurVPN#
> ```
