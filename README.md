# Mise en place d’une plateforme de détection d’intrusions
## Introduction
Ce projet consiste à créer une plateforme de détection des intrusions. Nous allons préparer un lab local pour répondre aux besoins de ce projet. GNS3 est un bon outil à utiliser pour la création de ce lab.

![GNS3](img/gns3.jpg)

Dans ce projet, le travail est divisé en trois parties :

- Étude et implémentation: une étude des attaques sur chaque couche (transport, internet, liaison de données). Cette étape inclut également l'implémentation de chaque attaque.
- Snort et Suricata: cette étape a pour objectif de faire une étude approfondie de Snort et Suricata, et d'analyser les différences entre les deux.
- Implémentation des attaques : la dernière étape consiste à implémenter les attaques et à les détecter à l'aide de Snort et Suricata.
## Etude et implémentation 
Étude et implémentation :
Pour la couche transport, nous étudierons trois attaques :

- Attaque SYN Flood : avec hping3
- Attaque UDP Flood : avec hping3
- Attaque TCP Reset : avec hping3 ou en utilisant le fichier tcp_reset.py

Pour la couche internet :

- Spoofing d'adresse IP : en utilisant nmap avec les options -S ou -D
- Attaques de routage : avec Loki ([Lien GitHub](https://github.com/Raizo62/Loki_on_Kali))
- Attaques d'injection de fausses informations de routage : avec injection_attack.py

Pour la couche d'accès réseau :

- ARP Spoofing : avec ettercap 
- Attaque de déauthentification (ou déconnexion) : avec airmon-ng
- MAC Spoofing : avec macchanger

## Snort & Suricata 
### Snort
Les etapes pour installer Snort :
