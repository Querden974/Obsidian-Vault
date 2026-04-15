Pour paramétrer le routage OSPF, sur les routeur utiliser la commande  `router ospf 1` puis comme RIP ajouter les réseaux. La différence avec RIP il est nécessaire de renseigner le wildcard et une `area` à la commande
```
router ospf 1
network 10.0.0.0 0.0.0.3 area 0
```
Une fois tout les réseaux renseigné, il est possible de voir les différentes routes avec `show ip route`
```
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
* - candidate default, U - per-user static route, o - ODR
P - periodic downloaded static route 

Gateway of last resort is not set

	10.0.0.0/30 is subnetted, 1 subnets
O      10.0.0.0 [110/2] via 14.0.0.2, 00:19:17, FastEthernet0/0
	11.0.0.0/30 is subnetted, 1 subnets
O      11.0.0.0 [110/3] via 14.0.0.2, 00:15:26, FastEthernet0/0
	13.0.0.0/30 is subnetted, 1 subnets
	14.0.0.0/30 is subnetted, 1 subnets
C      14.0.0.0 is directly connected, FastEthernet0/0
C      192.168.3.0/24 is directly connected, FastEthernet0/1
```

Pour redistribuer les routes statiques ou dynamiques:
``` cisco
router ospf 1
redistribute static        <-- Redistribue les routes statiques
redistribute rip subnet    <-- Redistribue les routes RIP
```
