---

---
PowerShell utilise des opérateurs sous forme de **mots-clés**, toujours **insensibles à la casse**.

---

# 🔹** Opérateurs d’égalité**

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-eq` | égal à | `"test" -eq "test"` |
| `-ne` | différent de | `"a" -ne "b"` |

---

# 🔹 **Comparaison numérique**

| Opérateur | Signification | Exemple |
| --- | --- | --- |
| `-gt` | greater than (>) | `5 -gt 3` |
| `-ge` | greater or equal (>=) | `5 -ge 5` |
| `-lt` | less than (<) | `2 -lt 4` |
| `-le` | less or equal (<=) | `3 -le 3` |

---

# 🔹 **Opérateurs de correspondance (wildcards, regex, etc.)**

## 🔸 Wildcards (jokers * et ?)

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-like` | correspondance simple | `"hello" -like "he*"` |
| `-notlike` | ne correspond pas | `"hello" -notlike "*world"` |

## 🔸 Expressions régulières (regex)

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-match` | correspondance regex | `"abc123" -match "\d+"` |
| `-notmatch` | regex ne correspond pas | `"abc" -notmatch "\d"` |

## 🔸 Remplacement regex (pour mémoire)

```powershell
"abc123" -replace "\d+", "XYZ"   # => "abcXYZ"
```

---

# 🔹 **Opérateurs de comparaison de chaînes**

### Comparaisons **sensibles à la casse**

Ajouter un **c** devant l’opérateur :

- `ceq` : égal sensible à la casse
- `cne`, `cgt`, `clt`, etc.

### Exemples

```powershell
"Test" -eq "test"   # True
"Test" -ceq "test"  # False (car sensible à la casse)
```

---

# 🔹 **Opérateurs logiques**

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-and` | ET | `($age -gt 18 -and $age -lt 65)` |
| `-or` | OU | `($role -eq "admin" -or $role -eq "superuser")` |
| `-not` | NON | `-not ($active)` |
| `!` | NON (alias) | `!$active` |

---

# 🔹 **Opérateurs de type**

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-is` | est de ce type | `"hello" -is [string]` |
| `-isnot` | n’est pas de ce type | `5 -isnot [string]` |

---

# 🔹 **Contient / Inclus / Subset**

Avec arrays ou collections :

| Opérateur | Description | Exemple |
| --- | --- | --- |
| `-contains` | la collection contient l’élément | `@(1,2,3) -contains 2` |
| `-notcontains` | ne contient pas | `@(1,2,3) -notcontains 5` |
| `-in` | l'élément est dans la collection | `2 -in @(1,2,3)` |
| `-notin` | n’est pas dans la collection | `"x" -notin @("a","b")` |

---

# 🔹 **Comparaisons sensibles / insensibles à la culture**

| Préfixe | Description |
| --- | --- |
| `-c` | *case-sensitive* |
| `-i` | *case-insensitive* (valeur par défaut) |

Exemples :

```powershell
"HELLO" -ieq "hello"   # True
"HELLO" -ceq "hello"   # False
```

---

# 🔹 **Comparateurs PowerShell vs Opérateurs classiques**

| PowerShell (objet) | Langage classique |
| --- | --- |
| `-eq` | == |
| `-ne` | != |
| `-gt` | > |
| `-lt` | < |
| `-like` | wildcards |
| `-match` | regex |