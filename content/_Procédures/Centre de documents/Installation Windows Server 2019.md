---
base: "[[_Centre de documents.base]]"
Catégorie:
  - Windows
Heure de la dernière modification: 2026-03-08T19:02:00
Dernière modification par: Gautier RAYEROUX
Créée par: Gautier RAYEROUX
Date de création: 2026-03-08T19:01:00
---
**Auteur :** Gautier Rayeroux  |  **Date :** 11/12/2025

---

## Prérequis minimaux

| CPU | RAM | Stockage | Réseau |
| --- | --- | --- | --- |
| • 1.4 GHz 64-bit<br>• Compatible x64<br>• Support NX et DEP<br>• Support CMPXCHG16b, LAHF/SAHF, PrefetchW<br>• Support SLAT (EPT ou NPT) | • 1 Go pour Server Core<br>• 2 Go pour Server avec Desktop Experience<br>• RAM ECC recommandée pour les hôtes physiques | 32 Go minimum | • Adaptateur Ethernet ≥ 1 Gbit/s<br>• Conforme à la spécification PCI Express |

---

## 1. Installation de l'OS

1. Sélectionner la **langue**, le **format horaire** et la **méthode d'entrée**, puis cliquer sur **« Suivant »**
![[imported-image-JcCG.png]]
2. Sélectionner l'**édition** de Windows Server à installer, puis cliquer sur **« Suivant »**
![[imported-image-Vp.png]]
3. Accepter le **contrat de licence**, puis cliquer sur **« Suivant »**
![[imported-image-bt.png]]
4. Choisir le type d'installation : sélectionner **« Personnalisée »** pour une nouvelle installation
![[imported-image-Oe.png]]
5. Sélectionner le **disque** sur lequel installer Windows Server, puis cliquer sur **« Suivant »**
Patienter jusqu'au redémarrage automatique de la machine.
![[imported-image-PM.png]]
6. Définir un **mot de passe** pour le compte Administrateur, puis terminer l'installation
![[imported-image-iBw.png]]

---

## 2. Configuration de l'adresse IP

Pour un serveur, évitez l'adressage automatique DHCP — configurez une adresse **IP statique**.

7. Ouvrir les **paramètres réseau** (Panneau de configuration → Réseau → Propriétés de la carte)
8. Dans les propriétés **TCP/IPv4**, définir une adresse IP statique, un masque de sous-réseau, une passerelle et un DNS
![[imported-image-UD.png]]