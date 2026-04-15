---

---
## Déclaration

```bash
name="John"
age=42readonly PI=3.14
```

⚠️ Pas d’espace autour du `=`

## Utilisation

```bash
echo"$name"echo"${name}"
```

Toujours mettre les variables entre guillemets.

## Variables spéciales

| Variable | Signification |
| --- | --- |
| `$0` | Nom du script |
| `$1..$9` | Arguments |
| `$@` | Tous les arguments |
| `$#` | Nombre d’arguments |
| `$?` | Code retour |
| `$$` | PID du script |

# 📥 Lecture d’arguments

```bash
echo"Arg1: $1"
```

Boucle sur tous :

```bash
for argin"$@";doecho"$arg"done
```

# 🧵 Substitution de commande

```bash
files=$(ls)
today=$(date)
```

# 🧹 Manipulation de chaînes

```bash
name="hello.txt"echo"${name%.txt}"# helloecho"${name#h}"# ello.txtecho"${#name}"# longueur
```

---

# 📚 Tableaux

```bash
arr=("a""b""c")echo"${arr[0]}"echo"${arr[@]}"
```

Boucle :

```bash
for ein"${arr[@]}";doecho"$e"done
```