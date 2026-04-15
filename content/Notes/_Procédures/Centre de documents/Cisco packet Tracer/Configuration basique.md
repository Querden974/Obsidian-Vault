---

---
# Définir un nom de switch

```plain text
Router> enable
Router# configure terminal
Router(config)# hostname R1
```

# Définir un mot de passe

```plain text
Router> enable
Router# configure terminal
Router(config)# enable secret cisco123
Router(config)# line console 0
Router(config-line)# password admin
Router(config-line)# login
Router(config-line)# exit
```

# Sauvegarder la configuration

```plain text
Router> enable
Router# configure terminal
Router# copy running-config starter-config
```
