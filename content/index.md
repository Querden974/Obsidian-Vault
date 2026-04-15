---
tags:
  - index
  - dashboard
  - tssr
---

# TSSR — Dashboard

> [!abstract] Vue d'ensemble
> Vault de notes de la formation **TSSR** (Technicien Supérieur Systèmes et Réseaux).
> Couvre l'administration système Windows/Linux, les réseaux Cisco, le scripting, la virtualisation et la supervision.

---

## Administration Windows

--- start-multi-column: AdminWindows 
```column-settings
number of columns: 2
Column Size: [50%, 50%]
```


> [!example] Active Directory & PowerShell
>
> - [[🧑‍💼 Active Directory (Niveau 1 et 2)]] — Concepts AD, utilisateurs, groupes, OUs
> - [[Notes/_Procédures/Centre de documents/Powershell/index]] — Scripts, AD, GPO, ACL, mode Core

--- end-column ---

> [!example] Outils & Références Windows
>
> - [[Commandes Windows]] — CMD et commandes système essentielles
> - [[🛠️ Utilitaires système indispensables]] — Outils de diagnostic et d'administration

--- end-multi-column 

---

## Scripting & Développement

--- start-multi-column: Scripting
```column-settings
number of columns: 3
Column Size: [33%, 33%, 33%]
```

> [!info] Shell Linux
>
> - [[Notes/_Procédures/Centre de documents/Bash/index]] — Variables, conditions, boucles, fonctions, trap

--- end-column ---

> [!info] Langages
>
> - [[Java]] — POO Java
> - [[PHP]] — Web côté serveur
> - [[Requêtes SQL]] — Bases de données relationnelles
> - [[NoSQL]] — Bases de données non relationnelles

--- end-column ---

> [!info] Outils Dev
>
> - [[Git]] — Versionnage et collaboration
> - [[Regex]] — Expressions régulières
> - [[Ansible]] — Automatisation et déploiement

--- end-multi-column

---

## Réseau

--- start-multi-column: Réseau
```column-settings
number of columns: 3
Column Size: [33%, 33%, 33%]
```

> [!note] Fondamentaux
>
> - [[Bases des Réseaux Informatiques]] — Modèles OSI/TCP-IP, protocoles
> - [[Calcul sous-réseau]] — CIDR, masques, découpage (FLSM)
> - [[VLSM - Découpage réseau]] — Découpage variable, astuces VLSM
> - [[Domaine de collision et de diffusion]] — Segmentation réseau

--- end-column ---

> [!note] VLANs & Routage
>
> - [[Paramétrer les VLAN]] — Création, accès, trunk
> - [[Utiliser les VLAN pour réduire les domaines de diffusion]]
> - [[Routage Inter-Vlan]] — Router-on-a-stick, SVI
> - [[Notes/_Procédures/Centre de documents/Cisco packet Tracer/index]] — Config IOS, DHCP, SSH, VLANs

--- end-column ---

> [!note] Routage dynamique
>
> Voir le centre de ressources Routage dans les procédures :
> - [[🦽Routage - RIP]] — Vecteur de distance, 15 sauts max
> - [[🚗Routage - EIGRP]] — Hybride Cisco, algorithme DUAL
> - [[🏍Routage - OSPF]] — État de liens, areas
> - [[Synthèse - Protocoles de Routage Dynamique]]

--- end-multi-column

---

## Infrastructure & Virtualisation

--- start-multi-column: Infra
```column-settings
number of columns: 3
Column Size: [33%, 33%, 33%]
```

> [!warning] Conteneurisation
>
> - [[_Docker]] — Images, conteneurs, Docker Compose

--- end-column ---

> [!warning] Déploiement
>
> - [[Notes/_Procédures/Centre de documents/Fog/index]] — Clonage réseau, PXE, images Windows

--- end-column ---

> [!warning] Automatisation
>
> - [[Ansible]] — Playbooks, inventaires, modules

--- end-multi-column

---

## Supervision

> [!info] Zabbix
> - [[Notes/_Procédures/Centre de documents/Zabbix/index]] — Supervision réseau et système, alertes, SNMP, LLD
>   - [[Installation serveur Zabbix]] · [[Installation agent Zabbix passif (Linux)]]
>   - [[SNMP Cisco]] · [[Sondes Personnalisées]] · [[Alertes et Notifications]]

---

## Procédures & Ressources

--- start-multi-column: Procédures
```column-settings
number of columns: 2
Column Size: [50%, 50%]
```

> [!tip] Procédures pas-à-pas
>
> - [[Administration Active Directory]] — Gestion AD en procédure guidée
> - [[Installation Windows Server 2019]]
> - [[Mise en place serveur DHCP]]
> - [[Mise en place d'un serveur DNS (Bind9)]]
> - [[Installer un serveur GLPI sous Ubuntu]]
> - [[Installation d'applications Docker]]
> - [[Mise en place d'une machine virtuelle]]
> - [[Connexion au bureau à distance]]
> - [[Mise en place d'un serveur temps avec Chrony]]

--- end-column ---

> [!tip] Références & Formation
>
> - [[Dictionnaire]] — Glossaire des termes IT
> - [[Plan Auto-Formation]] — Plan de montée en compétences sur 4 semaines
> - [[📋 To-Do List – Implémentation d'un serveur sur Cisco Packet Trace]]

--- end-multi-column
