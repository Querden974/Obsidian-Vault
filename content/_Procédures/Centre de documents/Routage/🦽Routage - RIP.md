# Caractéristiques du RIPv2
- Le protocole de routage RIP fait partie des **protocoles de routage de vecteur de distance**.
- Sa distance administrative est égal à 120 (utile si plusieurs protocoles de routage sont utilisés, ça permet au routeur d’utilisé la route la plus rapide pour arriver à destination)
- **La métrique utilisée est le nombre de saut** (1 routeur = 1 saut)
- Le nombre de **saut maximum est de 15**, à partir de 16 routeurs le paquet est perdu.
- Trois instances de temporisation
    - Mise à jour de la table de routage toutes les 30 secondes
    - Temporisation d’invalidation = 180 secondes sans nouvelle de cette route, le routeur marque le routeur de destination injoignable
    - Temporisation d’effacement = 240 secondes sans nouvelle de la route injoignable, le routeur l’efface de sa table de routage au bout de 240s.
- Envoi **ses mises de routage sur toutes les interfaces du routeur par défaut**, et envoi la totalité de sa table de routage

# Mise en place RIP avec Cisco Packet Tracer
Effectuer la mise en place de plusieurs routeurs et des ordinateurs et renseigner les adresses IP de chaque interfaces.
![[Routage - RIP-1775036683537.jpeg]]
Pour chaque routeur, aller dans la console pour pouvoir activer le protocole RIPv2 et définir les réseaux qui vont être accessible par la le protocole.
```
Router#config terminal
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#no auto-summary
Router(config-router)#network 10.0.0.12
Router(config-router)#network 10.0.0.16
Router(config-router)#network 10.0.0.0
Router(config-router)#end
```
Une fois que tout les routeurs ont été configuré avec les réseaux autorisés, il maintenant possible de visualiser les différentes routes du protocole RIP.
```
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
		D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
		N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
		E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
		i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
		* - candidate default, U - per-user static route, o - ODR
		P - periodic downloaded static route

Gateway of last resort is not set

	10.0.0.0/30 is subnetted, 5 subnets
C      10.0.0.0 is directly connected, Serial3/0
R      10.0.0.4 [120/1] via 10.0.0.2, 00:00:20, Serial3/0
R      10.0.0.8 [120/1] via 10.0.0.14, 00:00:25, Serial2/0
C      10.0.0.12 is directly connected, Serial2/0
C      10.0.0.16 is directly connected, Serial6/0
	172.16.0.0/24 is subnetted, 1 subnets
R      172.16.0.0 [120/1] via 10.0.0.14, 00:00:25, Serial2/0
R      192.168.10.0/24 [120/1] via 10.0.0.18, 00:00:22, Serial6/0
```

Si on décide de couper le lien entre le R1 et R2 (voir schéma), dans la table de routage, la métrique va changer et va afficher \[120/3] ce qui signifie que pour accéder à ce réseau, il est nécessaire de faire 3 sauts.

L'avantage du routage dynamique va nous permettre d'avoir des routes différentes lorsqu'une route devient inaccessible. 

Il est possible de définir une route statique par défaut sur un routeur et de pouvoir la propager, pour ça, sur un routeur, définir une route statique
	`ip route 0.0.0.0 0.0.0.0 10.0.0.9`
Puis une fois la route créée, aller dans les paramétrage de routage RIP et activer la redistribution statique.
```
Router> enable
Router# configure terminal
Router(config)# router rip
Router(config-router)# redistribute static
```
Sur les autres routeurs, lors de la visualisation de la table de routage, on peut voir une route par défaut ajouté dynamiquement ce qui va permettre de créer un chemin vers l'ISP.

Pour redistribuer les routes dynamiques qui proviennent d'un autre protocole OSPF ou EIGRP :
```
router rip
redistribute static        <-- Redistribue les routes statiques
redistribute ospf 1 metric 5    <-- Redistribue les routes RIP
```
