---

---
## 🔹 Déterminer les adresses de tous les sous-réseaux

### 1️⃣ Identifier le masque étendu

- Réseau de départ : `200.35.1.0/24`
- Masque étendu choisi : `/27`
- Masque décimal : `255.255.255.224`

---

### 2️⃣ Trouver l’octet qui varie

- Le masque `/27` agit sur le **4ᵉ octet**
- Ce sont donc les valeurs du **dernier octet** qui changent

---

### 3️⃣ Calculer l’incrément

Formule à retenir :

```plain text
Incrément = 256 − valeur du masque dans l’octet concerné
```

Application :

```plain text
256 − 224 = 32
```

---

### 4️⃣ Lister les adresses de sous-réseaux

On part de `0` et on ajoute l’incrément (`32`) à chaque fois :

```plain text
0, 32, 64, 96, 128, 160, 192, 224
```

Adresses réseau obtenues :

```plain text
200.35.1.0/27
200.35.1.32/27
200.35.1.64/27
200.35.1.96/27
200.35.1.128/27
200.35.1.160/27
200.35.1.192/27
200.35.1.224/27
```

---

## 🔹Déterminer l’adresse de diffusion d’un sous-réseau

### 🔸 Règle essentielle

> L’adresse de diffusion est l’adresse juste avant le sous-réseau suivant

---

### Exemple : sous-réseau n°2

- Adresse réseau du sous-réseau n°2 :
`200.35.1.32`
- Adresse réseau du sous-réseau suivant :
`200.35.1.64`

---

### Calcul de l’adresse de diffusion

```plain text
Adresse de diffusion = 200.35.1.63
```

---

## 🧠 Mémo rapide à retenir

```plain text
Sous-réseaux → incrément constant
Adresse réseau → début du bloc
Adresse de diffusion → réseau suivant − 1
```