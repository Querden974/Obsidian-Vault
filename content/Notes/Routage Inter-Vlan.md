---

---
> [!note]+ ## Créer les VLAN dans le switch de niveau 3
> ```plain text
> SwitchL3(config)# vlan 20
> SwitchL3(config-vlan)# name Direction
> SwitchL3(config-vlan)# exit
> ```
> 
> > [!warning] ⚠️
> > Effectuer les commandes pour chaque VLAN

> [!note]+ ## Créer les liaisons Trunks et autoriser les VLAN sur ces liaisons Trunks
> ```plain text
> SwitchL3(config)# interface g1/0/2
> SwitchL3(config-if)# switchport trunk encapsulation dot1q
> SwitchL3(config-if)# switchport mode trunk
> SwitchL3(config-if)# switchport trunk native vlan 100
> SwitchL3(config-if)# switchport trunk allowed vlan 
> 20,21,40,50,100
> SwitchL3(config-if)# exit
> ```
> 
> > [!warning] ⚠️
> > Effectuer les commandes pour chaque port afin de créer les liaisons Trunks et autoriser les VLAN sur les liaisons Truncks

> [!note]+ ## Affecter les VLAN dans les différentes interfaces.
> ```plain text
> SwitchL3(config)# int g1/0/6
> SwitchL3(config-if)# switchport mode access
> SwitchL3(config-if)# switchport access vlan 30
> SwitchL3(config-if)# exit
> ```
> 
> > [!warning] ⚠️
> > Effectuer les commandes pour chaque port qui ne nécessite pas de liaisons Trunks

> [!note]+ ## Créer les interfaces visuelles (SVI)
> ```plain text
> SwitchL3(config)# interface vlan 20
> SwitchL3(config-if)# description Passerelle SVI Direction
> SwitchL3(config-if)# ip address 192.168.20.254 255.255.255.0
> SwitchL3(config-if)# ipv6 address 2001:db8:acad:20::254/64
> SwitchL3(config-if)# no shutdown
> SwitchL3(config-if)# exit
> ```
> 
> > [!warning] ⚠️
> > Effectuer les commandes pour chaque VLAN avec l’adresse passerelle par défaut adéquat
> > Ne pas oublier le `no shutdown`  [ l’interface passe en état actif (si le câble et l’autre extrémité sont OK). ] 
> 
> > [!note] 📢
> > Étape importante pour la communication entre les différents VLANS
