---
Créée par: Maxime COURBOULIN
base: "[[_Centre de documents.base]]"
Catégorie:
  - Windows
---
**_Procédure changement adresse IP_**

Désactiver DHCP pou passer en adresse IP statique sur Windows 10 professionnel

Vérifier DHCP activé :

- terminal : ipconfig /all
- DHCP activé : oui

Une fois vérification effectuée :

- W+r : ncpa.cpl

![Changement_Adresse_IP](<Changement_Adresse_IP.png>)

- « Ethernet » : click droit -> propriété
- Protocole Interne Version 4 -> propriété
- ![Changement_Adresse_IP](<Changement_Adresse_IP 1.png>)
- Changer IP et DNS
- ![Changement_Adresse_IP](<Changement_Adresse_IP 2.png>)