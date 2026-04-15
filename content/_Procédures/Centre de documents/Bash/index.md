---
tags:
  - bash
  - scripting
  - linux
  - index
Catégorie:
  - Linux
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T00:00:00
base: "[[_Centre de documents.base]]"
---

# Bash — Centre de Ressources

> [!abstract] Présentation
> **Bash** (Bourne Again SHell) est le shell par défaut sur la plupart des distributions Linux. Il permet d'automatiser des tâches système via des scripts, en combinant commandes, variables, structures de contrôle et redirections. Tout script robuste commence par :
> ```bash
> #!/usr/bin/env bash
> set -euo pipefail
> IFS=$'\n\t'
> ```
> `set -e` arrête le script en cas d'erreur · `set -u` interdit les variables non définies · `set -o pipefail` propage les erreurs dans les pipes

---

## Fondamentaux

> [!example] Bases
> ### [[📦 Basics]]
> Variables, tableaux et manipulation de chaînes.
>
> - Déclaration : `name="val"` (pas d'espace autour de `=`)
> - Toujours citer : `"$var"` ou `"${var}"`
> - `readonly PI=3.14` — constante
> - Manipulation de chaînes : `${name%.txt}`, `${#name}`
> - Tableaux : `arr=("a" "b")` → `${arr[@]}`

> [!example] Conditions
> ### [[🔀 Conditions]]
> Tests et branchements conditionnels.
>
> - `if [[ ... ]]; then ... elif ... else ... fi`
> - Toujours utiliser `[[ ]]` plutôt que `[ ]`
> - Comparaisons chaînes : `=`
> - Comparaisons entiers : `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`
> - Logique : `&&`, `||`, `!`

> [!example] Boucles
> ### [[🔁 Boucles]]
> Itérer sur des valeurs, des fichiers ou des flux.
>
> ```bash
> for i in 1 2 3; do ... done
> for file in *.txt; do ... done
> while read -r line; do ... done < file.txt
> until ping -c1 host; do sleep 1; done
> ```

---

## Structures de contrôle

> [!info] Case (switch)
> ### [[🧩 Case (switch)]]
> Remplacer les chaînes de `if/elif` par un `case` lisible.
>
> ```bash
> case "$1" in
>   start) echo "Start" ;;
>   stop)  echo "Stop"  ;;
>   *)     echo "Usage: start|stop" ;;
> esac
> ```

> [!info] Fonctions
> ### [[🧱 Fonctions]]
> Découper un script en blocs réutilisables.
>
> ```bash
> my_func() {
>   local name="$1"   # local est CRUCIAL
>   echo "Hello $name"
> }
> result=$(my_func "John")
> ```
>
> - `local` pour éviter la pollution de l'espace global
> - `return` pour un code de retour (0–255)
> - `echo` + `$()` pour retourner une valeur string

---

## Techniques avancées

> [!warning] Gestion d'erreurs — trap
> ### [[🧪 Try - Catch en BASH (avec trap)]]
> Bash n'a pas de `try/catch` natif — on simule avec `trap`.
>
> ```bash
> trap 'echo "Erreur ligne $LINENO"; exit 1' ERR
> set -e
> ```
>
> Ou gestion manuelle :
> ```bash
> if ! cp file.txt /tmp/; then
>   echo "Erreur copie"
> fi
> ```

> [!warning] Substitution de commande
> ### [[🧵 Substitution de commande]]
> Capturer la sortie d'une commande dans une variable.
>
> ```bash
> files=$(ls)
> today=$(date)
> count=$(wc -l < fichier.txt)
> ```
>
> Toujours préférer `$(...)` à l'ancienne syntaxe avec backticks.

---

## Rappels essentiels

### Variables spéciales

| Variable | Signification |
|---|---|
| `$0` | Nom du script |
| `$1` … `$9` | Arguments positionnels |
| `$@` | Tous les arguments (liste) |
| `$#` | Nombre d'arguments |
| `$?` | Code retour de la dernière commande |
| `$$` | PID du script en cours |
| `$LINENO` | Numéro de ligne courant |

### Redirections

| Syntaxe | Signification |
|---|---|
| `>` | Réécrire stdout vers un fichier |
| `>>` | Ajouter stdout à un fichier |
| `2>` | Rediriger stderr |
| `&>` | Rediriger stdout + stderr |
| `\|` | Pipeline — passer stdout à la commande suivante |
| `/dev/null` | Supprimer toute sortie |

---

> [!tip] Structure minimale d'un script robuste
> ```bash
> #!/usr/bin/env bash
> set -euo pipefail
> IFS=$'\n\t'
>
> # --- Fonctions ---
> usage() { echo "Usage: $0 <argument>"; exit 1; }
> trap 'echo "Erreur ligne $LINENO"; exit 1' ERR
>
> # --- Vérification des arguments ---
> [[ $# -lt 1 ]] && usage
>
> # --- Corps du script ---
> echo "Argument reçu : $1"
> ```
>
> 1. Déclarer les variables et fonctions → [[📦 Basics]] et [[🧱 Fonctions]]
> 2. Gérer les cas d'erreur avec `trap` → [[🧪 Try - Catch en BASH (avec trap)]]
> 3. Tester les conditions et les fichiers → [[🔀 Conditions]]
> 4. Itérer sur les données → [[🔁 Boucles]]
> 5. Dispatcher avec `case` pour les menus → [[🧩 Case (switch)]]
> 6. Capturer les sorties de commandes → [[🧵 Substitution de commande]]
