---

---
Bash n’a pas de try/catch natif, on simule :

```bash
trap'echo "Erreur ligne $LINENO"; exit 1' ERRset -e
```

Ou gérer manuellement :

```bash
if !cp file.txt /tmp/;thenecho"Erreur copie"fi
```
