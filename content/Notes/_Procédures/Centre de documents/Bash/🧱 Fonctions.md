---

---
```bash
my_func() {local name="$1"echo"Hello $name"
}

my_func"John"
```

`local` est CRUCIAL dans les fonctions.

Retourner une valeur :

```bash
return 1# code retourecho"valeur"# sortie
```

Récupérer :

```bash
result=$(my_func)
```