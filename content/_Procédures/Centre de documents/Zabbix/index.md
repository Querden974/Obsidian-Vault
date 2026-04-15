---
tags:
  - zabbix
  - monitoring
  - index
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-04-08T13:32:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T13:32:00
---

# Zabbix — Centre de Ressources

> [!abstract] Présentation
> **Zabbix** est une solution open-source de supervision réseau et système. Elle permet de surveiller la disponibilité, les performances et l'intégrité des infrastructures IT en temps réel, avec alertes, graphiques et tableaux de bord.
> 
> Port agent passif : **10050/TCP** | Port serveur : **10051/TCP** | Interface web : **HTTP/HTTPS**

---

## Installation & Configuration

> [!example] Serveur
> ### [[Installation serveur Zabbix]]
> Installation complète du serveur Zabbix sur Debian avec MariaDB, Nginx et l'interface web.
> 
> - Dépôt Zabbix + paquets
> - Base de données MariaDB
> - Configuration Nginx
> - Interface web & premier utilisateur

> [!example] Agent
> ### [[Installation agent Zabbix passif (Linux)]]
> Déploiement d'un agent passif sur une machine Linux à superviser.
> 
> - Installation du paquet agent
> - Configuration `zabbix_agentd.conf`
> - Ouverture pare-feu (port 10050)
> - Déclaration de l'hôte dans l'UI

---

## Supervision & Collecte

> [!info] Protocole SNMP
> ### [[SNMP Cisco]]
> Supervision d'équipements réseau Cisco CBS 250 via SNMP depuis Zabbix.
> 
> - Activation SNMP sur le switch
> - Community string
> - Template SNMP dans Zabbix
> - Port UDP 161

> [!info] Métriques personnalisées
> ### [[Sondes Personnalisées]]
> Étendre les capacités de l'agent avec des UserParameters.
> 
> - Définition de `UserParameter`
> - Scripts de collecte custom
> - Intégration dans les items Zabbix
> - Tests avec `zabbix_agentd -t`

---

## Découverte & Prototypes

> [!info] Prototypes LLD (Item, Trigger, Graph)
> ### [[Prototypes LLD (Item, Trigger, Graph)]]
> Créer automatiquement des items, triggers et graphiques via le **Low-Level Discovery**.
>
> - Règle de découverte (clé `vfs.fs.discovery`, `net.if.discovery`…)
> - Item prototype avec macros `{#NOM}`
> - Trigger prototype avec sévérité et expression
> - Graph prototype multi-métriques

---

## Alertes & Réactivité

> [!warning] Alertes et Notifications
> ### [[Alertes et Notifications]]
> Configurer des triggers et des actions pour être notifié en cas d'incident.
> 
> ```
> Trigger déclenché → Action évaluée → Opération exécutée → Notification envoyée
> ```
> 
> - Création et gestion des triggers
> - Sévérités : Information / Warning / Average / High / Disaster
> - Actions : e-mail, webhook, script
> - Fenêtres de maintenance

---

## Rappels essentiels

| Élément | Valeur par défaut |
|---|---|
| Identifiants par défaut | `Admin` / `zabbix` |
| Port agent (passif) | `10050/TCP` |
| Port serveur | `10051/TCP` |
| Interface web | `http://<IP>:<port>` |
| Config agent | `/etc/zabbix/zabbix_agentd.conf` |
| Config serveur | `/etc/zabbix/zabbix_server.conf` |
| Logs agent | `/var/log/zabbix/zabbix_agentd.log` |
| Logs serveur | `/var/log/zabbix/zabbix_server.log` |

---

> [!tip] Flux de mise en place rapide
> 1. Installer le **serveur** → [[Installation serveur Zabbix]]
> 2. Déployer un **agent** sur chaque hôte → [[Installation agent Zabbix passif (Linux)]]
> 3. Ajouter les hôtes dans l'**interface web** et assigner un template
> 4. Configurer les **triggers** et **alertes** → [[Alertes et Notifications]]
> 5. Créer des **sondes custom** si besoin → [[Sondes Personnalisées]]
