---

---
# Docker Cheatsheet 🐳

[[Dockerfile  Docker-Compose]]

## **Concepts Clés**

- **Image** : Modèle immuable utilisé pour créer un conteneur.
- **Conteneur** : Instance en cours d’exécution d’une image.
- **Dockerfile** : Fichier contenant les instructions pour construire une image.
- **Registry** : Dépôt pour stocker des images Docker (ex : Docker Hub).

---

---

<!-- Column 1 -->
## **Commandes Générales**

### Afficher la version de Docker

```shell
docker --version
```

### Lister les commandes disponibles

```shell
docker help
```


<!-- Column 2 -->

## **Images**

### Télécharger une image depuis Docker Hub

```shell
docker pull <nom_image>:<tag>
# Exemple : docker pull nginx:latest
```

### Lister les images locales

```shell
docker images
```

### Supprimer une image locale

```shell
docker rmi <image_id>
```

---

<!-- Column 1 -->
## **Conteneurs**

### Créer et démarrer un conteneur

```shell
docker run <options> <nom_image>
# Options courantes :
# -d : Démarre le conteneur en arrière-plan
# -p : Mappe un port local à un port du conteneur (ex : -p 8080:80)
# --name : Donne un nom au conteneur
# -v : Monte un volume local (ex : -v /chemin/local:/chemin/conteneur)
```

### Lister les conteneurs

```shell
docker ps          # Conteneurs en cours d’exécution
docker ps -a       # Tous les conteneurs (actifs et stoppés)
```

### Arrêter un conteneur

```shell
docker stop <nom_ou_id_conteneur>
```

### Supprimer un conteneur

```shell
docker rm <nom_ou_id_conteneur>
```

### Accéder au shell d’un conteneur

```shell
docker exec -it <nom_ou_id_conteneur> /bin/bash
```


<!-- Column 2 -->
## **Volumes**

### Lister les volumes

```shell
docker volume ls
```

### Créer un volume

```shell
docker volume create <nom_volume>
```

### Supprimer un volume

```shell
docker volume rm <nom_volume>
```

---

## **Réseaux**

### Lister les réseaux

```shell
docker network ls
```

### Créer un réseau

```shell
docker network create <nom_reseau>
```

### Connecter un conteneur à un réseau

```shell
docker network connect <nom_reseau> <nom_ou_id_conteneur>
```


---

<!-- Column 1 -->
## **Docker Compose**

### Lancer des services définis dans un fichier `docker-compose.yml`

```shell
docker-compose up
# Options :
# -d : Démarrer en arrière-plan
```

### Arrêter les services

```shell
docker-compose down
```


<!-- Column 2 -->
## **Gestion des Ressources**

### Nettoyer les conteneurs, images et volumes inutilisés

```shell
docker system prune -a
```

### Vérifier l’utilisation des ressources

```shell
docker stats
```

---

## **Construire une Image**

### Créer une image à partir d’un `Dockerfile`

```shell
docker build -t <nom_image>:<tag> <chemin_du_Dockerfile>
# Exemple : docker build -t monapp:1.0 .
```

---

## **Exemples Pratiques**

### Démarrer un serveur web Nginx

```shell
docker run -d -p 8080:80 --name nginx-server nginx:latest
```

### Créer un conteneur avec un volume

```shell
docker run -d -v $(pwd)/data:/data --name volume-test busybox
```

---

### Conseils 🚀

1. **Privilégiez **`**docker-compose**` pour des configurations complexes.
2. **Utilisez des tags spécifiques** (évitez `latest`) pour éviter des versions inattendues.
3. **Versionnez vos **`**Dockerfile**` dans votre dépôt de code pour des déploiements reproductibles.