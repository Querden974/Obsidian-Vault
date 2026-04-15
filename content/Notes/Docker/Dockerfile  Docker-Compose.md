---

---
<!-- Column 1 -->
## **Dockerfile**

Un `Dockerfile` est un fichier texte contenant des instructions pour créer une image Docker. C’est comme une recette pour construire votre application.

### **Structure de base d’un Dockerfile**

1. **Définir une image de base**
Exemple :
```plain text
FROM node:16
```
2. **Copier les fichiers dans l’image**
Exemple :
```plain text
COPY . /app
WORKDIR /app
```
3. **Installer des dépendances**
Exemple :
```plain text
RUN npm install
```
4. **Exposer un port (optionnel)**
Exemple :
```plain text
EXPOSE 3000
```
5. **Définir la commande à exécuter au démarrage**
Exemple :
```plain text
CMD ["npm", "start"]
```

### **Commandes utiles**

- Construire une image :
```shell
docker build -t monapp:1.0 .
```
- Utiliser l’image :
```shell
docker run -p 3000:3000 monapp:1.0
```

<!-- Column 2 -->
## **Docker Compose**

`docker-compose.yml` est un fichier pour définir et gérer plusieurs conteneurs en même temps. Il simplifie les choses, surtout si vous avez plusieurs services (ex : backend, frontend, base de données).

### **Structure de base d’un fichier **`**docker-compose.yml**`

6. **Déclarer une version**
Exemple :
```yaml
version: "3.9"
```
7. **Définir les services**
Exemple :
```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    command: npm start
```
8. **Ajouter une base de données (exemple)**
Exemple :
```yaml
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

### **Commandes utiles**

- Lancer les services :
```shell
docker-compose up
```
- Arrêter les services :
```shell
docker-compose down
```
- Recréer les conteneurs après modification :
```shell
docker-compose up --build
```

---

## **Exemple Complet**

### Dockerfile

```plain text
FROM node:16
WORKDIR /app
COPY . /app
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

### docker-compose.yml

```yaml
version: "3.9"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    command: npm start

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

### Étapes :

9. **Créer un **`**Dockerfile**`** et un **`**docker-compose.yml**`**.**
10. **Lancer avec :**
```shell
docker-compose up
```
11. **Votre application est prête sur **`**http://localhost:3000**`**.**