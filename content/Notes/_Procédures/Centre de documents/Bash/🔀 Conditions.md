---

---
## if

```bash
if [["$age" -gt 18 ]];thenecho"Adulte"elif [["$age" -eq 18 ]];thenecho"Pile 18"elseecho"Mineur"fi
```

## Tests importants

| Test | Signification |
| --- | --- |
| `-f file` | fichier existe |
| `-d dir` | dossier existe |
| `-z str` | chaîne vide |
| `-n str` | chaîne non vide |
| `str1 = str2` | égalité |
| `int1 -eq int2` | égalité int |

Toujours utiliser `[[ ... ]]` plutôt que `[ ... ]`.

<!-- Column 1 -->
# 🧵 Comparaisons sur **chaînes de caractères**

| Condition | Signification | Exemple |
| --- | --- | --- |
| `[[ "$a" = "$b" ]]` | égalité | `[[ "$name" = "john" ]]` |
| `[[ "$a" != "$b" ]]` | différent |   |
| `[[ -z "$a" ]]` | chaîne vide |   |
| `[[ -n "$a" ]]` | chaîne non vide |   |
| `[[ "$a" < "$b" ]]` | ordre alphabétique |   |
| `[[ "$a" > "$b" ]]` | ordre alphabétique |   |
| `[[ "$a" =~ regex ]]` | match regex | `[[ "$email" =~ @gmail\.com$ ]]` |

⚠️ `<` et `>` ne marchent **que dans **`**[[ ]]**`

<!-- Column 2 -->
# 🔢 Comparaisons sur **entiers**

| Condition | Signification | Exemple |
| --- | --- | --- |
| `-eq` | égal | `[[ "$a" -eq 5 ]]` |
| `-ne` | différent |   |
| `-lt` | < |   |
| `-le` | ≤ |   |
| `-gt` | > |   |
| `-ge` | ≥ |   |

# 🧠 Logique booléenne

| Opérateur | Signification | Exemple |
| --- | --- | --- |
| `&&` | ET | `[[ -f f && -r f ]]` |
| `||` | OU | `[[ -z "$a" || -z "$b" ]]` |
| `!` | NON | `[[ ! -f f ]]` |


<!-- Column 3 -->
# 📁 Tests sur **fichiers / dossiers**

| Condition | Signification |
| --- | --- |
| `-e file` | existe |
| `-f file` | fichier normal |
| `-d dir` | dossier |
| `-L file` | lien symbolique |
| `-r file` | lisible |
| `-w file` | modifiable |
| `-x file` | exécutable |
| `-s file` | non vide |
| `file1 -nt file2` | plus récent |
| `file1 -ot file2` | plus ancien |

