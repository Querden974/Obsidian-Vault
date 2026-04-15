---

---
# 🪟 Cheatsheet – Commandes Windows utiles en maintenance informatique

| Commande | Description |
| --- | --- |
| `ipconfig` | Affiche la configuration IP du poste (adresse, passerelle, DNS) |
| `ipconfig /all` | Affiche les détails réseau complets (MAC, DHCP, etc.) |
| `ipconfig /release` / `renew` | Libère/renouvelle l'adresse IP via DHCP |
| `ping [adresse]` | Vérifie si une machine est joignable (latence réseau) |
| `tracert [adresse]` | Affiche le chemin emprunté jusqu'à une adresse (troubleshooting réseau) |
| `nslookup [nom]` | Vérifie la résolution DNS d'un nom de domaine |
| `netstat` | Affiche les connexions réseau actives et les ports utilisés |
| `tasklist` | Liste des processus en cours (équivalent du Gestionnaire des tâches) |
| `taskkill /PID [id] /F` | Termine un processus par son PID |
| `shutdown /r /t 0` | Redémarre immédiatement le PC |
| `whoami` | Affiche l’utilisateur actuellement connecté |
| `net user` | Liste les comptes utilisateurs locaux |
| `net user [nom] *` | Change le mot de passe d’un utilisateur |
| `net use` | Affiche ou connecte des lecteurs réseau |
| `net use Z: \\\\serveur\\partage` | Monte un dossier réseau sur une lettre |
| `sfc /scannow` | Vérifie l’intégrité des fichiers système Windows |
| `chkdsk` | Vérifie l’état du disque dur (système de fichiers) |
| `getmac` | Affiche les adresses MAC du PC |
| `hostname` | Affiche le nom de la machine |
| `systeminfo` | Donne les infos système (OS, build, RAM, etc.) |
| `gpupdate /force` | Applique immédiatement les stratégies de groupe |
| `services.msc` | Ouvre la gestion des services Windows |
| `msconfig` | Lance l’outil de configuration du système |
| `regedit` | Ouvre l’éditeur de registre |
| `eventvwr` | Ouvre la visionneuse d’événements Windows |
| `control` | Ouvre le panneau de configuration classique |
| `compmgmt.msc` | Ouvre la console de gestion de l'ordinateur |
| `devmgmt.msc` | Ouvre le gestionnaire de périphériques |

---