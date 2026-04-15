---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-04-15T08:22:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T08:22:00
---
# Installation serveur DNS
## a. Installer bind9

``` bash
sudo apt install -y bind9
```

## b. Sauvegarde des fichier de base de bind9

Créer une sauvegarde des fichier `named.conf.options` et `named.local`
``` sh
cd /etc/bind  
cp named.conf.options named.conf.options.bkp  
cp named.conf.local named.conf.local.bkp
```

## c. Configuration globale de Bind

```
// Autorise uniquement certains réseaux à soliciter le DNS
acl "lan" {
    192.168.92.0/24;
    localhost;
    localnets;
  };

options {
        // Répertoire de travail de Bind
        directory "/var/cache/bind";

        // Redirecteurs DNS (résolveurs externes)
        forwarders {
                8.8.8.8;
        };

        // Mode récursif, pour résoudre les noms externes 
        recursion yes;

        // Active la validation DNSSEC (vérifier l'authenticité des réponses DNS signées)
        dnssec-validation auto;

        // Ecouter sur toutes les interfaces réseau en IPv4 et IPv6
        listen-on { any; };
        listen-on-v6 { any; };
		
		// Autorise les requete des hôtes du réseau "lan"
        allow-query { lan; };
};
```

Vérifier la configuration avec la commande `named-checkconf`.

## d. Création d'une zone DNS

Déclarer une nouvelle zone DNS dans le fichier `/etc/bind/named.conf.local`
```
zone "fog.local" {
    type master;
    file "/etc/bind/db.fog.local";
    allow-update { none; };
};
```

Dupliquer le fichier `db.local` pour l'utiliser comme base de la nouvelle zone DNS.
``` sh
sudo cp /etc/bind/db.local /etc/bind/db.fog.local
```

> [!warning] Important
> Si le fichier `db.local` n'existe pas, créer un fichier qui porte le nom de la zone `db.fog.local`

Editer le fichier de configuration de la zone:
```
; BIND data file for fog.local
$TTL    604800
@       IN      SOA     srv-dns.fog.local. admin.fog.local. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      srv-dns.fog.local.
srv-dns         IN      A       192.168.92.147
```

Tester la configuration de la zone avec la commande suivante:
```
named-checkzone fog.local /etc/bind/db.fog.local 
```
## e. Démarrer Bind9

``` sh
sudo systemctl start bind9  
sudo systemctl enable named.service  
sudo systemctl status bind9
```

## f. Tester la résolution de nom

Modifier le fichier `/etc/resolv.conf` et indiquer le nom de domaine et l'adresse:
```
search fog.local
domain fog.local
nameserver 127.0.0.1
```

Effectuer un `nslookup` sur le nom de domaine pour tester la résolution.