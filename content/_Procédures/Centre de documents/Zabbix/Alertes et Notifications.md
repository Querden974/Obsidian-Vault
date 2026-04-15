---
tags:
  - zabbix
  - alertes
  - notifications
  - trigger
---

# Alertes et Notifications Zabbix

## Principe général

```
Trigger déclenché → Action évaluée → Opération exécutée → Notification envoyée
```

Zabbix nécessite **3 éléments** pour envoyer une alerte :

| Élément | Rôle | Où configurer |
|---|---|---|
| **Media type** | Canal d'envoi (email, Slack…) | Administration > Media types |
| **Utilisateur** | Destinataire + canal associé | Administration > Users |
| **Action** | Règle "si trigger X → envoyer à Y" | Configuration > Actions > Trigger actions |

---

## Étape 1 — Configurer Teams
Aller sur une équipe Teams, puis cliquer sur l'icone d'option du canal de discussion.
![[Alertes et Notifications-1776080623070.jpeg]]
Chercher le model `Envoyer des alertes webhook à un canal`  et choisir l'équipe et le canal souhaité.
![[Alertes et Notifications-1776080746452.jpeg]]
Copier le lien webhook pour configurer les alertes Zabbix.
Exemple:
``` 
https://default8fb4b9bceaa84d95a84a8b68c0321a.a2.environment.api.powerplatform.com:443/powerautomate/automations/direct/workflows/259f013a863f41359d76daaa327d39d6/triggers/manual/paths/invoke?api-version=1&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=rZlU7oVm7edRV360doEcB_j2iF9lbaPpMS36aeAStco
```

---
## Étape 2 — Configurer un Media Type

### MS Teams Workflow

*Administration > Media types > MS Teams Workflow*

Modifier les paramètres suivants:

| Nom            | Valeur                                                                                                                                                                                                                                                                                             |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| teams_endpoint | https://default8fb4b9bceaa84d95a84a8b68c0321a.a2.environment.api.powerplatform.com:443/powerautomate/automations/direct/workflows/259f013a863f41359d76daaa327d39d6/triggers/manual/paths/invoke?api-version=1&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=rZlU7oVm7edRV360doEcB_j2iF9lbaPpMS36aeAStco |
| zabbix_url     | http://192.168.255.75:8080                                                                                                                                                                                                                                                                         |
> [!warning] Changer les valeurs en fonction des conditions

> [!tip] Zabbix URL
> Il est recommandé de faire une nouvelle macro pour la variable `zabbix_url` et d'associer l'adresse depuis la page macro.

Cliquer **Test** pour vérifier l'envoi avant de sauvegarder.
![[Alertes et Notifications-1776081257487.jpeg]]

![[Alertes et Notifications-1776081329934.jpeg]]

---

## Étape 3 — Créer une Action

*Configuration > Actions > Trigger actions > Create action*

### Onglet Action

| Champ | Valeur |
|---|---|
| Nom | `Alerte critique serveurs` |
| Conditions | Voir ci-dessous |
| Enabled | Coché |

### Conditions (filtrage des triggers)

Exemples de conditions cumulables (opérateur **And/Or**) :

| Condition | Opérateur | Valeur |
|---|---|---|
| Trigger severity | `>=` | `High` |
| Host group | `equals` | `Serveurs Linux` |
| Trigger | `does not equal` | `(triggers à ignorer)` |
| Maintenance status | `not in` | `maintenance` |

> [!tip]+ Commencer simple
> Pour un premier test, ne mettre **aucune condition** : l'action s'appliquera à tous les triggers. Affiner ensuite.

### Onglet Operations — Envoi lors du déclenchement

Cliquer **Add** dans la section *Operations* :

| Champ                  | Valeur                                   |
| ---------------------- | ---------------------------------------- |
| Send to users / groups | Sélectionner l'utilisateur ou le groupe  |
| Send only to           | `MS Teams Workflow` (ou `All`)           |
| Default message        | Coché (utilise le message du Media type) |

**Message personnalisé** (décocher *Default message*) :

```
Sujet : [{TRIGGER.SEVERITY}] {TRIGGER.NAME} sur {HOST.NAME}

Hôte    : {HOST.NAME} ({HOST.IP})
Trigger : {TRIGGER.NAME}
Gravité : {TRIGGER.SEVERITY}
Statut  : {TRIGGER.STATUS}
Heure   : {EVENT.DATE} {EVENT.TIME}
Valeur  : {ITEM.VALUE}

URL : {TRIGGER.URL}
```

### Onglet Recovery operations — Notification de résolution

Cliquer **Add** dans *Recovery operations* pour notifier quand le problème se résout :

```
Sujet : [RÉSOLU] {TRIGGER.NAME} sur {HOST.NAME}

Le problème est résolu à {EVENT.RECOVERY.DATE} {EVENT.RECOVERY.TIME}.
Durée : {EVENT.DURATION}
```

### Onglet Update operations (optionnel)

Permet d'envoyer une notification quand un utilisateur **acquitte** le problème manuellement dans Zabbix.

---

## Macros utiles dans les messages

| Macro | Valeur retournée |
|---|---|
| `{HOST.NAME}` | Nom de l'hôte |
| `{HOST.IP}` | Adresse IP |
| `{TRIGGER.NAME}` | Nom du trigger |
| `{TRIGGER.SEVERITY}` | Gravité (Warning, High…) |
| `{TRIGGER.STATUS}` | PROBLEM ou OK |
| `{TRIGGER.URL}` | URL configurée dans le trigger |
| `{ITEM.NAME}` | Nom de l'item |
| `{ITEM.VALUE}` | Dernière valeur collectée |
| `{EVENT.DATE}` | Date de l'événement |
| `{EVENT.TIME}` | Heure de l'événement |
| `{EVENT.DURATION}` | Durée du problème |
| `{EVENT.RECOVERY.DATE}` | Date de résolution |

---

## Vérification et test

### Forcer un test d'alerte

1. Aller sur *Monitoring > Problems*
2. Si aucun problème n'est visible, **baisser temporairement le seuil** d'un trigger existant pour le déclencher
3. Vérifier que la notification arrive

### Consulter les logs d'envoi

*Reports > Action log* — affiche chaque tentative d'envoi avec le statut (`Sent` / `Failed`) et le message d'erreur éventuel.

*Administration > Media types > [Media type] > Test* — test d'envoi direct sans trigger.

---

## Résumé — Checklist

- [ ] Media type configuré et testé (Administration > Media types)
- [ ] Media associé à l'utilisateur avec les bonnes gravités (Administration > Users > Media)
- [ ] Action créée avec les conditions souhaitées (Configuration > Actions > Trigger actions)
- [ ] Opération d'envoi définie (Send to user/group)
- [ ] Recovery operation ajoutée (notification de résolution)
- [ ] Test effectué via *Action log* ou déclenchement manuel
