---

---
## 🏗 Structure de Base

Les regex sont entourées de `/` et peuvent inclure des modificateurs :

```javascript
const regex = /motif/flags;
```

**Exemple :**

```javascript
const regex = /hello/i;  // Recherche "hello" en ignorant la casse
console.log(regex.test("HELLO")); // true
```

---

---

<!-- Column 1 -->
## 🔍 Modificateurs (Flags)

| Modificateur | Description |
| --- | --- |
| `g` | Global (toutes les occurrences) |
| `i` | Ignore la casse (maj/min) |
| `m` | Multiligne (`^` et `$` fonctionnent par ligne) |
| `s` | `.` capture aussi les retours à la ligne |
| `u` | Active le support Unicode |
| `y` | Recherche ancrée (sticky) |

**Exemple :**

```javascript
const regex = /chat/gi;
console.log("Chat chat CHat".match(regex)); // ["Chat", "chat", "CHat"]
```

**Négation :**

| Symbole | Signification |
| --- | --- |
| `\D` | Tout sauf un chiffre (`[^0-9]`) |
| `\W` | Tout sauf une lettre/chiffre (`[^a-zA-Z0-9_]`) |
| `\S` | Tout sauf un espace |
| `\B` | Pas une limite de mot |

## 📏 Quantificateurs

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `*` | 0 ou plusieurs fois | `/a*/` → "", "a", "aaa" |
| `+` | 1 ou plusieurs fois | `/a+/` → "a", "aaa" (pas "") |
| `?` | 0 ou 1 fois (optionnel) | `/colou?r/` → "color" ou "colour" |
| `{n}` | Exactement `n` fois | `/a{3}/` → "aaa" |
| `{n,}` | Au moins `n` fois | `/a{2,}/` → "aa", "aaaa" |
| `{n,m}` | Entre `n` et `m` fois | `/a{2,4}/` → "aa", "aaa", "aaaa" |

## 🔀 Alternance (`|`)

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `a | b` | "a" ou "b" |

## 🚀 Ancres de Position (`^` & `$`)

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `^` | Début de chaîne | `/^Hello/` → "Hello world" |
| `$` | Fin de chaîne | `/world$/` → "Hello world" |

<!-- Column 2 -->

## 🛠 Métacaractères

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `.` | N'importe quel caractère sauf retour à la ligne | `/c.t/` → "cat", "cot", "cut" |
| `\d` | Chiffre (`0-9`) | `/\d+/` → "42", "123" |
| `\w` | Lettre, chiffre ou `_` | `/\w+/` → "hello_123" |
| `\s` | Espace blanc | `/\s+/` → " " (espace, tab, saut de ligne) |
| `\b` | Délimitation de mot | `/\bchat\b/` → "chat", mais pas "château" |
| `\n` | Retour à la ligne | `/\n/` |
| `\t` | Tabulation | `/\t/` |

## 🎯 Classes de Caractères

| Classe | Signification | Exemple |
| --- | --- | --- |
| `[abc]` | "a", "b" ou "c" | `/[aeiou]/` → Trouve une voyelle |
| `[a-z]` | Lettre minuscule de "a" à "z" | `/[a-z]/` |
| `[A-Z]` | Lettre majuscule de "A" à "Z" | `/[A-Z]/` |
| `[0-9]` | Chiffres de "0" à "9" | `/[0-9]/` |
| `[^abc]` | Tout sauf "a", "b" ou "c" | `/[^aeiou]/` → Trouve une consonne |

## 🔄 Groupes & Captures

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `(abc)` | Capture le groupe "abc" | `/(hello)/` |
| `(?:abc)` | Groupe sans capture | `/(?:hello)/` |
| `\1` | Référence à un groupe capturé | `/(\w+) \1/` → "hello hello" |

**Exemple :**

```javascript
const regex = /(\d{2})-(\d{2})-(\d{4})/;
const date = "12-05-2023".match(regex);
console.log(date[1]); // "12" (jour)
console.log(date[2]); // "05" (mois)
console.log(date[3]); // "2023" (année)
```

## 🔧 Remplacement avec `replace()`

```javascript
const texte = "123-ABC";
const regex = /\d+/;
console.log(texte.replace(regex, "XXX")); // "XXX-ABC"
```

## 🎯 Exemples Pratiques

<!-- Column 1 -->
### ✅ Vérifier une adresse e-mail

```javascript
const regex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
console.log(regex.test("email@example.com")); // true
```


<!-- Column 2 -->
### ✅ Filtrer un input pour accepter uniquement des lettres

```javascript
document.getElementById("textInput").addEventListener("input", function () {
    this.value = this.value.replace(/[^a-zA-ZÀ-ÿ\s]/g, "");
});
```
