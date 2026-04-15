---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Docker
  - Linux
Heure de la dernière modification: 2026-03-08T19:11:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T18:54:00
---
**Auteur :** Gautier  |  **Date :** 03/02/2026

---

## Prérequis

- Navigation sur un OS Linux en CLI
- Connaissances en adressage réseau
- Virtualisation type 1 ou 2

Dans cette procédure, une machine virtuelle sous **Debian 13** est utilisée.

---

## 1. Configuration de l'adresse IP

1. Sur Linux en CLI, la configuration réseau se fait via le fichier `interfaces`. Ouvrir le fichier avec Nano :
```plain text
nano /etc/network/interfaces
```
![[imported-image-sIw.png]]
![[imported-image-zE.png]]
Repérer le nom de l'interface principale (ex. : `ens33`) et son mode actuel (DHCP).
2. Pour passer en adresse statique, remplacer `dhcp` par `static` et renseigner les paramètres réseau (IP, passerelle, masque…)
![[imported-image-NW.png]]
3. Sauvegarder avec **Ctrl+O** → **Entrée**, puis quitter avec **Ctrl+X**
![[imported-image-HHG.png]]
![[imported-image-QxY.png]]
4. Redémarrer le service réseau :
```plain text
systemctl restart networking
```
Vérifier l'adresse appliquée :
```plain text
ip a
```
![[imported-image-SjE.png]]
5. Ajouter un serveur DNS :
```plain text
nano /etc/resolv.conf
```
Ajouter la ligne :
```plain text
nameserver 8.8.8.8
```
![[imported-image-GGt.png]]
Tester la connectivité :
```plain text
ping deb.debian.org
```

---

## 2. Installer Docker

6. Ajouter le dépôt apt officiel de Docker :
```plain text
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```
7. Installer les paquets Docker :
```plain text
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 3. Créer un conteneur Docker

### 3.1 Via ligne de commande (CLI)

```plain text
docker run -d --name it-tools --restart unless-stopped -p 8080:80 <nom-image>:latest
```

- `d` — Exécute en arrière-plan (detached)
- `-name` — Nom du conteneur
- `-restart unless-stopped` — Redémarre automatiquement sauf arrêt manuel
- `p 8080:80` — Redirige le port 8080 de la machine vers le port 80 du conteneur
- `:latest` — Dernière version de l'image

### 3.2 Via Docker Compose (recommandé)

L'indentation est **obligatoire** dans les fichiers `.yml`.

8. Créer un répertoire dédié au conteneur, puis créer un fichier `docker-compose.yml`
9. Exemple de configuration minimale :
```plain text
services:
  it-tools:
    image: 'corentinth/it-tools:latest'
    ports:
      - '8090:80'
    restart: 'unless-stopped'
    container_name: it-tools
```
10. Démarrer le conteneur :
```plain text
docker compose up -d
```

---

## 4. Installation BentoPDF

11. Créer un dossier dédié et y créer le fichier `docker-compose.yml` :
```plain text
services:
  bentopdf:
    image: bentopdfteam/bentopdf-simple:latest
    container_name: bentopdf
    restart: unless-stopped
    ports:
      - '8080:8080'
    environment:
      - DISABLE_IPV6=true
```
12. Démarrer le conteneur :
```plain text
docker compose up -d
```
13. Accéder à BentoPDF via l'IP de la machine et le port configuré (ex. : `192.168.92.10:8080`)
![[imported-image-XadN.png]]

---

## 5. Installation It-Tools

14. Créer un dossier dédié et y créer le fichier `docker-compose.yml` :
```plain text
services:
  it-tools:
    image: 'corentinth/it-tools:latest'
    ports:
      - '8090:80'
    restart: 'unless-stopped'
    container_name: it-tools
```
15. Démarrer le conteneur :
```plain text
docker compose up -d
```
16. Accéder à It-Tools via `<IP-machine>:8090`