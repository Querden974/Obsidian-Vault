---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Linux
Heure de la dernière modification: 2026-04-15T08:20:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-04-15T08:20:00
---
# Installation serveur DHCP
## a. Installer le serveur
``` bash
sudo apt install -y isc-dhcp-server
```


## b. Configurer le serveur

Récupérer le nom de l'interface avec la commande `ip a` avant d'ouvrir le fichier de configuration (exemple `ens33`)

Editer le fichier de configuration du serveur DCHP `/etc/default/isc-dhcp-server`
```
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf     # <-- Retirer #
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="ens33"     <-- Définir le nom de l'interface
INTERFACESv6=""
```

## c. Créer une étendue DHCP

Configurer la plage DHCP avec le fichier `/etc/dhcp/dhcpd.conf`:
```
option domain-name "fog.local";

default-lease-time 600;
max-lease-time 7200;

# Serveur DHCP principal sur ce réseau local
authoritative;

ddns-update-style none;

# Plage DHCP
subnet 192.168.92.0 netmask 255.255.255.0 {
    range 192.168.92.200 192.168.92.220;
    option domain-name-servers 192.168.92.147;
    option routers 192.168.92.2;
  }
```

Démarrer le service et activer au démarrage:
``` bash
sudo systemctl start isc-dhcp-server 
sudo systemctl enable isc-dhcp-server
```
