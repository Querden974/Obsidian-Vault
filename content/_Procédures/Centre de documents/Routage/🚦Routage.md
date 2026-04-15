
| [[🦽Routage - RIP]] | [[🏍Routage - OSPF]] | [[🚗Routage - EIGRP]]<br> |
| ------------------- | :------------------: | ------------------------- |
# **1. `no auto-summary`**

C'est le plus important. Sans cette commande, EIGRP agrège automatiquement les routes en classes (classful), ce qui cause des problèmes de routage avec des adresses non contiguës. C'est d'ailleurs ce que tu vois dans ta table de routage avec les routes `Null0` :

```
D 12.0.0.0/8 is a summary, 03:03:36, Null0
```

Ces routes vers `Null0` sont dangereuses — elles absorbent les paquets sans les transmettre. À ajouter systématiquement :

```
router eigrp 1
 no auto-summary
```

---

# **2. `passive-interface`**

Sur les interfaces connectées aux PCs ou réseaux terminaux, EIGRP envoie des messages Hello inutilement. Il faut les bloquer :

```
router eigrp 1
 passive-interface FastEthernet0/0
```

---

# **3. `router-id` (bonne pratique)**

Comme OSPF, il est conseillé de fixer un router-id manuel pour éviter les comportements imprévisibles :

```
router eigrp 1
 eigrp router-id 1.1.1.1
```