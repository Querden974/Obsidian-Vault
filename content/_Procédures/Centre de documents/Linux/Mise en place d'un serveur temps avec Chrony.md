---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-03-08T19:06:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T19:04:00
---
**Auteur :** Gautier Rayeroux  |  **Date :** 24/02/2026

---

## Prérequis

Avoir une **VM hôte** (serveur NTP) et une **VM client** présentes dans le **même réseau**.

---

## 1. Mise en place du serveur Chrony

### 1.1 Installation

Sur la machine qui va accueillir le serveur NTP, exécuter les commandes suivantes :

```plain text
sudo apt update && sudo apt upgrade -y
sudo apt install chrony -y
```

### 1.2 Configuration

Ouvrir le fichier de configuration de Chrony :

```plain text
nano /etc/chrony/chrony.conf
```

Effectuer la modification suivante :

| 🔴 Chercher la ligne | 🟢 Remplacer par |
| --- | --- |
| pool 2.debian.pool.ntp.org iburst | local stratum 10 |

Le paramètre `local stratum 10` configure cette machine comme source de temps locale de référence, même sans accès à Internet.

### 1.3 Redémarrage et vérification

Redémarrer le service Chrony :

```plain text
sudo systemctl restart chrony
```

Vérifier la configuration :

```plain text
chronyc tracking
```

Résultat attendu :

Reference ID : 7F7F0101
Stratum : 10

---

## 2. Installation du client Chrony

### 2.1 Installation

Sur la machine cliente, exécuter les commandes suivantes :

```plain text
sudo apt update && sudo apt upgrade -y
sudo apt install chrony -y
```

### 2.2 Configuration

Ouvrir le fichier de configuration de Chrony :

```plain text
nano /etc/chrony/chrony.conf
```

Effectuer la modification suivante (remplacer `192.168.92.130` par l'IP de votre serveur NTP) :

| 🔴 Chercher la ligne | 🟢 Remplacer par |
| --- | --- |
| pool 2.debian.pool.ntp.org iburst | server 192.168.92.130 iburst |

### 2.3 Redémarrage et vérification

Redémarrer le service Chrony :

```plain text
sudo systemctl restart chrony
```

Tester la connexion avec le serveur Chrony :

```plain text
chronyc sources
```

Résultat attendu :

```plain text
MS Name/IP address Stratum Poll Reach LastRx Last sample
===============================================================================
^* 192.168.92.130 10 9 377 295 +6913ns[+7421ns] +/- 356us
```

Vérifier le suivi détaillé :

```plain text
chronyc tracking
```

Résultat attendu :

```plain text
Reference ID : C0A85C82 (192.168.92.130)
Stratum : 11
Ref time (UTC) : Tue Feb 24 14:02:45 2026
System time : 0.000003559 seconds slow of NTP time
Last offset : -0.000011561 seconds
RMS offset : 0.000019739 seconds
Frequency : 0.002 ppm slow
Residual freq : -0.001 ppm
Skew : 0.022 ppm
Root delay : 0.000173915 seconds
Root dispersion : 0.000855934 seconds
Update interval : 522.1 seconds
Leap status : Normal
```

La ligne **Reference ID** affiche l'adresse IP du serveur NTP — cela confirme que le client est bien synchronisé avec le serveur Chrony.