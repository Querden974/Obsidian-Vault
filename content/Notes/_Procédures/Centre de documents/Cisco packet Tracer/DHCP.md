---

---
```plain text
Router>enable
Router#configure terminal
Router(config)#ip dhcp pool CLIENT_LAN
Router(dhcp-config)#network 192.168.0.0 255.255.255.0
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#default-router 192.168.0.1
```

- `**ip dhcp pool**`** **: nom de l'étendue (pool), ce qui permet de s'y retrouver s'il y a plusieurs pools configurés sur un même routeur.
- `**network**` : adresse du réseau, le routeur va distribuer toutes les adresses IP disponibles.
- `**dns-server**` : serveur DNS à distribuer.
- `**default-router**` : passerelle par défaut à distribuer.

### Exclure une adresse

```plain text
Router(dhcp-config)#exit
Router(config)#ip dhcp excluded-address 192.168.0.240 192.168.0.250
```
