---

---
# Créer des VLAN

Sur un switch dans CISCO Packet Tracker entrer en mode<u> </u><u>**configuration du terminal**</u> puis utilisez ces commandes pour créer un VLAN et lui donner un nom.

```plain text
Dir-Exam(config)# vlan 20
Dir-Exam(config-vlan)# name Direction
Dir-Exam(config-vlan)# exit
```

# Attribuer des VLAN

Pour attribuer un VLAN à une interface, il faut taper les commandes suivantes :

```plain text
Dir-Exam(config)# interface fa0/1
Dir-Exam(config-if)# switchport mode access
Dir-Exam(config-if)# switchport access vlan 20
Dir-Exam(config-if)# exit
```

> [!tip] 
> Effectuer les commandes pour chaque port interface nécessaire

# Créer les interfaces visuelles (SVI)

```plain text
SwitchL3(config)# interface vlan 20
SwitchL3(config-if)# description Passerelle SVI Direction
SwitchL3(config-if)# ip address 192.168.20.254 255.255.255.0
SwitchL3(config-if)# ipv6 address 2001:db8:acad:20::254/64
SwitchL3(config-if)# no shutdown
SwitchL3(config-if)# exit
```

> [!warning] 
> Effectuer les commandes pour chaque VLAN avec l’adresse passerelle par défaut adéquat
> Ne pas oublier le `no shutdown`  [ l’interface passe en état actif (si le câble et l’autre extrémité sont OK). ] 

> [!note] 
> Étape importante pour la communication entre les différents VLANS

# Configurer la téléphonie sur IP

> [!tip] 
> Nécessite de créer un VLAN dédié a la téléphonie en amont

Dans la configuration de l’interface où se situe le téléphone VoIP, il va falloir:

```plain text
Dir-Exam(config)# int f0/1
Dir-Exam(config-if)# mls qos trust cos
Dir-Exam(config-if)# switchport voice vlan 50
Dir-Exam(config-if)# exit

Dir-Exam(config)# int f0/2
Dir-Exam(config-if)# mls qos trust cos
Dir-Exam(config-if)# switchport voice vlan 50
Dir-Exam(config-if)# exit
```

# **Vérifiez des informations sur les VLAN**

Une fois qu’un VLAN est configuré, les configurations VLAN peuvent être validées à l’aide des commandes IOS de Cisco `**show vlan**`** ou **`**show vlan brief**`** **qui affichera un résumé.

On peut également afficher des précisions sur les VLAN en affichant des informations sur les interfaces, en utilisant la commande `**show interfaces**`**. (ex: **`**show interfaces fa0/1 switchport**`** )**

# **Modifiez l’appartenance d’un VLAN**

Il existe plusieurs façons de modifier l’appartenance des ports aux VLAN. Si le port d’accès au commutateur a été attribué de manière incorrecte à un VLAN, il vous suffit de saisir à nouveau la commande de configuration de l’interface `**switchport access vlan**`** ***vlan-id* avec l’ID VLAN correct.

Pour modifier l’appartenance d’un port à un VLAN, utilisez la commande `**no switchport access vlan**`.

> [!warning] 
> Si vous enlevez l’appartenance d’un VLAN à un port, le VLAN est toujours actif bien qu’il ne soit plus associé à l’interface.

# **Supprimez le VLAN**

La commande `**no vlan**`** ***vlan-id* est utilisée pour supprimer un VLAN du fichier `**vlan.dat**`, c’est ce fichier qui contient la liste des VLAN créés.

> [!warning] 
> Avant de supprimer un VLAN, réattribuez d’abord tous les ports membres à un VLAN différent. Tous les ports qui ne sont pas déplacés vers un VLAN actif sont incapables de communiquer avec d’autres hôtes après la suppression du VLAN. La communication ne pourra se faire qu’une fois les ports attribués à un VLAN actif.

L’ensemble du fichier vlan.dat peut être supprimé à l’aide de la commande `**delete flash:vlan.dat**` en mode d’accès privilégié. La version abrégée de la commande (`**delete vlan.dat**`) peut être utilisée si le fichier vlan.dat n’a pas été déplacé de son emplacement par défaut.

Après l’exécution de cette commande et le redémarrage du commutateur, les VLAN précédemment configurés ne sont plus présents. Cette commande rétablit les paramètres d’usine par défaut du commutateur, en ce qui concerne les configurations de VLAN.

# **Effectuez un Trunk de VLAN**

Maintenant que vous avez configuré et vérifié les VLAN, il est temps de configurer et de vérifier les Trunks VLAN.

> [!tip] 
> Un **Trunk de VLAN** est un lien de couche 2 entre deux commutateurs, qui achemine le trafic **pour tous les VLAN** (à moins que la liste des VLAN autorisés ne soit restreinte manuellement ou dynamiquement).

Pour activer la liaison Trunk d’un commutateur, configurez le port d’interconnexion avec l’ensemble des commandes de configuration d’interface suivantes:

```plain text
Dir-Exam(config)# interface g0/1
Dir-Exam(config-if)# switchport mode trunk
Dir-Exam(config-if)# switchport trunk native vlan 100
Dir-Exam(config-if)# switchport trunk allowed vlan 20,21,40,50,100
Dir-Exam(config-vlan)# end
```

> [!note] 
> Refaire ces commandes pour chaque commutateur avec les bon VLAN

Pour réinitialiser le port Trunk à l’état par défaut, utilisez les commandes `**no switchport allowed vlan**` et `**no switchport trunk native vlan**` pour supprimer les VLAN autorisés et réinitialiser le VLAN natif du Trunk.

Lorsqu’il est remis à l’état par défaut, le Trunk autorise tous les VLAN, et utilise le VLAN 1 comme VLAN natif.