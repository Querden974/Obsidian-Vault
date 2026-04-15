Pour paramétrer le routage EIGRP, sur les routeur utiliser la commande  `router eigrp 1` puis comme OSPF ajouter les réseaux avec les wildcards. 
```
router eigrp 1
network 10.0.0.0 0.0.0.3
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
	11.0.0.0/30 is subnetted, 1 subnets
	12.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D 12.0.0.0/8 is a summary, 03:03:36, Null0
C 12.0.0.0/30 is directly connected, Serial0/0/0
13.0.0.0/30 is subnetted, 1 subnets
	14.0.0.0/30 is subnetted, 1 subnets
	15.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D 15.0.0.0/8 is a summary, 03:03:44, Null0
C 15.0.0.0/30 is directly connected, FastEthernet0/0
	16.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D 16.0.0.0/8 is a summary, 03:03:44, Null0
C 16.0.0.0/30 is directly connected, FastEthernet0/1
D 17.0.0.0/8 [90/2172416] via 15.0.0.2, 03:03:49, FastEthernet0/0
			 [90/2172416] via 16.0.0.2, 03:03:43, FastEthernet0/1
D 192.168.1.0/24 [90/30720] via 16.0.0.2, 03:03:43, FastEthernet0/1
D 192.168.2.0/24 [90/30720] via 15.0.0.2, 03:03:49, FastEthernet0/0
```
> Ici on va retrouver la Distance Administrative `90` qui correspond à EIGRP et contrairement à RIPv2 qui va compter les sauts, mais la bande passante et le délai du chemin. Plus la métrique est basse, meilleur est le chemin.

Pour redistribuer les routes statiques ou dynamiques:
``` cisco
router eigrp 1
redistribute static        <-- Redistribue les routes statiques
redistribute rip metric 10000 10 255 1 150   <-- Redistribue les routes RIP
redistribute ospf 1 metric 10000 100 255 1 150   <-- Redistribue les routes OSPF
```

Il est important pour de la redistribution avec EIGPR d'ajouter plusieurs valeurs de métriques pour son fonctionnement:
- **`10000` — Bande passante (Bandwidth)** C'est le débit du lien en **Kb/s**. Plus la valeur est élevée, plus le lien est rapide. EIGRP l'utilise pour calculer le meilleur chemin.
- **`10` — Délai (Delay)** C'est le délai de transmission en **unités de 10 microsecondes**. Représente le temps que met un paquet à traverser le lien. Plus c'est bas, mieux c'est.
- **`255` — Fiabilité (Reliability)** C'est le taux de fiabilité du lien sur une échelle de **0 à 255**, où 255 = 100% fiable. Un lien qui ne perd jamais de paquets aura 255.
- **`1` — Charge (Loading)** C'est le taux d'utilisation du lien sur une échelle de **1 à 255**, où 255 = 100% chargé. On met 1 pour indiquer que le lien est **quasiment libre**.
- **`1500` — MTU (Maximum Transmission Unit)** C'est la taille maximale d'un paquet en **octets** que le lien peut transporter. La valeur standard Ethernet est 1500 octets.


> [!information]
> 
>💡 En pratique, EIGRP n'utilise que la **bande passante** et le **délai** pour calculer sa métrique par défaut. Les trois autres paramètres (fiabilité, charge, MTU) sont rarement modifiés.
```
IP-EIGRP neighbors for process 1

H Address Interface Hold Uptime SRTT RTO Q  Seq
						(sec)   (ms)    Cnt Num
0 12.0.0.2 Se0/0/0 12 03:24:35 40 1000 0 32
```
