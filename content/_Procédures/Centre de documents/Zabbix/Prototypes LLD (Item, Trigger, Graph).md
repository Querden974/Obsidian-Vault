---
tags:
  - zabbix
  - lld
  - prototype
  - discovery
---

# Prototypes LLD — Item, Trigger et Graph

> [!abstract] Qu'est-ce que le LLD ?
> Le **Low-Level Discovery (LLD)** est un mécanisme Zabbix qui détecte automatiquement des éléments dynamiques sur un hôte (interfaces réseau, disques, bases de données…) et crée des items, triggers et graphiques à la volée à partir de **prototypes**.
>
> ```
> Règle de découverte → Découverte des éléments → Création automatique des prototypes
> ```

---

## Prérequis

- Serveur Zabbix opérationnel et hôte ajouté
- Une **règle de découverte (Discovery Rule)** configurée sur l'hôte
- Accès à l'interface web avec droits d'administration

---

## Étape 1 — Créer une règle de découverte

Avant de pouvoir créer des prototypes, une règle de découverte doit exister.

1. Aller dans **Data collection → Hosts**
2. Cliquer sur **Discovery** sur la ligne de l'hôte cible
3. Cliquer sur **Create discovery rule**

| Champ | Valeur exemple | Description |
|---|---|---|
| **Name** | `Discover filesystems` | Nom lisible de la règle |
| **Type** | `Zabbix agent` | Source de la découverte |
| **Key** | `vfs.fs.discovery` | Clé retournant un JSON avec les macros |
| **Update interval** | `1h` | Fréquence de découverte |

> [!note] Macros LLD
> La clé de découverte retourne un tableau JSON contenant des **macros LLD** (ex. `{#FSNAME}`, `{#IFNAME}`). Ces macros sont ensuite utilisées dans les prototypes pour nommer dynamiquement chaque élément créé.

4. Cliquer sur **Add** pour enregistrer la règle

---

## Étape 2 — Créer un Item prototype

Un **Item prototype** définit la métrique à collecter pour chaque élément découvert.

### Accès

Dans la liste des règles de découverte, cliquer sur **Item prototypes** sur la ligne de la règle, puis sur **Create item prototype**.

### Configuration

| Champ | Valeur exemple | Description |
|---|---|---|
| **Name** | `Free space on {#FSNAME}` | Nom avec macro LLD |
| **Type** | `Zabbix agent` | Type de collecte |
| **Key** | `vfs.fs.size[{#FSNAME},free]` | Clé avec macro LLD |
| **Type of information** | `Numeric (float)` | Format de la valeur |
| **Units** | `B` | Unité affichée |
| **Update interval** | `5m` | Fréquence de collecte |

> [!tip] Utilisation des macros
> Les macros LLD `{#NOM}` sont remplacées automatiquement par les valeurs découvertes. Par exemple, si la règle découvre `/`, `/boot` et `/home`, trois items distincts seront créés.

### Onglets complémentaires

- **Preprocessing** : transformer la valeur brute (multiplication, regex, JSON path…)
- **Tags** : ajouter des étiquettes pour filtrer dans les dashboards
- **Custom intervals** : planifier des collectes ponctuelles

Cliquer sur **Add** pour enregistrer.

---

## Étape 3 — Créer un Trigger prototype

Un **Trigger prototype** définit une condition d'alerte pour chaque élément découvert.

### Accès

Dans la règle de découverte, cliquer sur **Trigger prototypes** → **Create trigger prototype**.

### Configuration

| Champ | Valeur exemple | Description |
|---|---|---|
| **Name** | `Low free space on {#FSNAME}` | Nom avec macro LLD |
| **Severity** | `Warning` / `High` / `Disaster` | Sévérité de l'alerte |
| **Expression** | *(voir ci-dessous)* | Condition de déclenchement |
| **Description** | `Espace disque < 10%` | Détail affiché dans l'alerte |

### Construire l'expression

Cliquer sur **Expression constructor** pour utiliser l'assistant graphique.

```
last(/Nom_hôte/vfs.fs.size[{#FSNAME},pfree])<10
```

> [!warning] Syntaxe des macros dans les expressions
> Les macros LLD doivent apparaître dans la **clé de l'item** référencé par le trigger, pas directement dans l'expression elle-même.

### Sévérités disponibles

| Sévérité | Couleur | Usage typique |
|---|---|---|
| Not classified | Gris | Inconnu |
| Information | Bleu | Informatif |
| Warning | Jaune | Surveiller |
| Average | Orange | Dégradation |
| High | Rouge | Impact fort |
| Disaster | Rouge foncé | Service hors ligne |

Cliquer sur **Add** pour enregistrer.

---

## Étape 4 — Créer un Graph prototype

Un **Graph prototype** génère automatiquement un graphique pour chaque élément découvert.

### Accès

Dans la règle de découverte, cliquer sur **Graph prototypes** → **Create graph prototype**.

### Configuration

| Champ | Valeur exemple | Description |
|---|---|---|
| **Name** | `Disk usage on {#FSNAME}` | Nom avec macro LLD |
| **Width / Height** | `900 / 200` | Dimensions en pixels |
| **Graph type** | `Normal` / `Stacked` / `Pie` | Type de graphique |
| **Y axis MIN/MAX** | `Fixed` ou `Calculated` | Échelle de l'axe Y |

### Ajouter des items au graphique

Dans la section **Items**, cliquer sur **Add** :

| Champ | Valeur | Description |
|---|---|---|
| **Item** | `vfs.fs.size[{#FSNAME},free]` | Item prototype à tracer |
| **Color** | `#00AA00` | Couleur de la courbe |
| **Draw style** | `Line` / `Filled` / `Bold line` | Style de tracé |
| **Y axis side** | `Left` / `Right` | Axe associé |

> [!tip] Combiner plusieurs métriques
> Il est possible d'ajouter plusieurs items dans un même graphique prototype (ex. espace libre + espace utilisé) pour une lecture plus complète.

Cliquer sur **Add** pour enregistrer.

---

## Étape 5 — Vérifier la découverte

1. Aller dans **Monitoring → Hosts** et cliquer sur l'hôte
2. Ouvrir l'onglet **Discovery** pour voir l'état de la règle
3. Attendre le prochain cycle de découverte (ou forcer via **Check now**)
4. Vérifier dans **Items**, **Triggers** et **Graphs** que les entrées ont bien été créées dynamiquement

> [!note] Délai de création
> Les prototypes sont instanciés **après le premier cycle de découverte**. Si rien n't apparaît, vérifier les logs : `tail -f /var/log/zabbix/zabbix_server.log`

---

## Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| Aucun item créé | Règle de découverte en erreur | Vérifier la clé et la connectivité agent |
| Macro non remplacée | Faute de frappe dans `{#NOM}` | Vérifier le nom exact retourné par la règle |
| Trigger jamais déclenché | Expression incorrecte | Tester via **Configuration → Hosts → Items → Check now** |
| Graphique vide | Item non collecté | Vérifier que l'item prototype collecte des données |
| Découverte trop lente | Intervalle trop long | Réduire l'**Update interval** de la règle |
