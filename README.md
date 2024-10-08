# Mise en place d’une plateforme de détection d’intrusions
## 1.Introduction
Ce projet consiste à créer une plateforme de détection des intrusions. Nous allons préparer un lab local pour répondre aux besoins de ce projet. GNS3 est un bon outil à utiliser pour la création de ce lab.

![GNS3](img/gns3.jpg)

Dans ce projet, le travail est divisé en trois parties :

- Étude et implémentation: une étude des attaques sur chaque couche (transport, internet, liaison de données). Cette étape inclut également l'implémentation de chaque attaque.
- Snort et Suricata: cette étape a pour objectif de faire une étude approfondie de Snort et Suricata, et d'analyser les différences entre les deux.
- Implémentation des attaques : la dernière étape consiste à implémenter les attaques et à les détecter à l'aide de Snort et Suricata.
## 2.Etude et implémentation 
Étude et implémentation :
Pour la couche transport, nous étudierons trois attaques :

- Attaque SYN Flood : avec ***hping3***
- Attaque UDP Flood : avec ***hping3***
- Attaque TCP Reset : avec ***hping3*** ou en utilisant le fichier ***tcp_reset.py***

Pour la couche internet :

- Spoofing d'adresse IP : en utilisant ***nmap*** avec les options ***-S*** ou ***-D***
- Attaques de routage : avec ***Loki*** ([Lien GitHub](https://github.com/Raizo62/Loki_on_Kali))
- Attaques d'injection de fausses informations de routage : avec injection_attack.py

Pour la couche d'accès réseau :

- ARP Spoofing : avec ***ettercap***
- Attaque de déauthentification (ou déconnexion) : avec ***airmon-ng***
- MAC Spoofing : avec ***macchanger***

## 3.Snort & Suricata 
### A.Snort
Voici les étapes pour installer Snort sur une distribution Linux, comme Ubuntu. Ces étapes couvrent l'installation, la configuration de base et la vérification de son bon fonctionnement.
#### Étape 1 : Mettre à jour votre système 
Avant d'installer Snort, il est conseillé de mettre à jour votre système pour vous assurer que tous les paquets sont à jour.

    sudo apt update 
    sudo apt upgrade 
    

#### Étape 2 : Installer les dépendances nécessaires
Snort nécessite certaines bibliothèques et outils. Installez-les avec les commandes suivantes :

    sudo apt install -y build-essential libpcap-dev libpcre3-dev libdumbnet-dev bison flex zlib1g-dev

#### Étape 3 : Télécharger et installer DAQ
Snort utilise DAQ (Data Acquisition Library) pour capturer des paquets. Téléchargez et installez-le avec les étapes suivantes :

    # Téléchargez DAQ
    wget https://www.snort.org/downloads/snort/daq-2.0.7.tar.gz

    # Extraire les fichiers
    tar -xvzf daq-2.0.7.tar.gz

    # Accédez au dossier DAQ
    cd daq-2.0.7

    # Compiler et installer DAQ
    ./configure
    make
    sudo make install
#### Étape 4 : Télécharger et installer Snort
Téléchargez la dernière version de Snort à partir du site officiel :

    # Téléchargez Snort
    wget https://www.snort.org/downloads/snort/snort-2.9.20.tar.gz

    # Extraire les fichiers
    tar -xvzf snort-2.9.20.tar.gz

    # Accédez au dossier Snort
    cd snort-2.9.20

    # Compiler et installer Snort
    ./configure --enable-sourcefire
    make
    sudo make install

#### Étape 5 : Configurer les répertoires
Créez les répertoires nécessaires pour les fichiers de configuration de Snort :

    sudo mkdir /etc/snort
    sudo mkdir /etc/snort/rules
    sudo mkdir /var/log/snort
    sudo mkdir /usr/local/lib/snort_dynamicrules

#### Étape 6 : Télécharger les règles de Snort
Les règles permettent à Snort de détecter les attaques ou comportements anormaux. Vous pouvez obtenir les règles sur le site de Snort après avoir créé un compte :

    # Télécharger les règles via snort.org
Une fois les règles téléchargées, placez-les dans le dossier  

    /etc/snort/rules
#### Étape 7 : Configurer Snort
Copiez le fichier de configuration par défaut vers le répertoire /etc/snort :

    sudo cp etc/snort.conf /etc/snort/
Modifiez le fichier de configuration /etc/snort/snort.conf selon vos besoins :

    sudo nano /etc/snort/snort.conf
Dans ce fichier, modifiez les chemins vers les règles :

    var RULE_PATH /etc/snort/rules
    var SO_RULE_PATH /usr/local/lib/snort_dynamicrules

#### Étape 8 : Tester l'installation
Vérifiez que Snort est correctement installé en exécutant une commande de test sur votre interface réseau. Remplacez eth0 par le nom de votre interface réseau :

    sudo snort -T -i eth0 -c /etc/snort/snort.conf

#### Étape 9 : Lancer Snort en mode écoute
Pour lancer Snort en mode IDS (Intrusion Detection System) :

    sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
Vous avez maintenant installé Snort.

### B.Suricata
Voici les étapes pour installer Suricata, un autre système de détection d'intrusions (IDS/IPS), sur une distribution Linux comme Ubuntu.
#### Étape 1 : Mettre à jour votre système
Avant d'installer Snort, il est conseillé de mettre à jour votre système pour vous assurer que tous les paquets sont à jour.
    
    sudo apt update
    sudo apt upgrade
#### Étape 2 : Installer les dépendances nécessaires
Suricata nécessite certaines bibliothèques pour fonctionner correctement. Installez-les avec la commande suivante :

    sudo apt install -y software-properties-common build-essential libpcap-dev libpcre3-dev \ zlib1g-dev libyaml-dev pkg-config libnet1-dev libcap-ng-dev libmagic-dev \ libjansson-dev libnss3-dev libgeoip-dev
#### Étape 3 : Ajouter le PPA de Suricata
Le moyen le plus simple d'installer Suricata est d'utiliser son PPA officiel.

    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt update
#### Étape 4 : Installer Suricata
Maintenant que le PPA est ajouté, installez Suricata avec la commande suivante :

    sudo apt install suricata
#### Étape 5 : Vérifier l'installation
Pour vérifier si Suricata est bien installé et connaître sa version, utilisez :
    
    suricata --build-info
#### Étape 6 : Configurer Suricata
Le fichier de configuration de Suricata se trouve dans /etc/suricata/suricata.yaml. Vous pouvez modifier ce fichier selon vos besoins. Voici quelques points importants à ajuster :
##### 1.Interfaces réseau
spécifiez l'interface réseau à surveiller (remplacez eth0 par votre interface).
    
    af-packet:
    - interface: eth0
##### 2.Chemin des règles
Suricata utilise des règles pour détecter les intrusions. Par défaut, les règles se trouvent dans ***/etc/suricata/rules/*** . Vous pouvez ajouter ou mettre à jour ces règles selon vos besoins.
#### Étape 7 : Télécharger et mettre à jour les règles de Suricata
Suricata utilise les règles de Emerging Threats pour détecter les menaces. Vous pouvez télécharger et installer ces règles avec les commandes suivantes :
    
    sudo apt install suricata-update
    sudo suricata-update
Vous pouvez également mettre à jour régulièrement les règles avec cette commande :
    
    sudo suricata-update
#### Étape 8 : Tester Suricata
Pour exécuter Suricata en mode IDS et commencer à surveiller le trafic réseau sur une interface donnée (par exemple eth0), utilisez la commande suivante :
    
    sudo suricata -c /etc/suricata/suricata.yaml -i eth0
#### Étape 9 : Surveiller les alertes
Les alertes de Suricata sont enregistrées dans des fichiers journaux, généralement dans ***/var/log/suricata/***. Pour visualiser les alertes en temps réel, vous pouvez utiliser :

    tail -f /var/log/suricata/fast.log
#### Étape 10 : Exécuter Suricata comme service
Suricata peut être configuré pour s'exécuter en tant que service sur votre machine. Utilisez les commandes suivantes pour démarrer, arrêter ou vérifier le statut du service Suricata :

    # Démarrer Suricata
    sudo systemctl start suricata

    # Activer Suricata au démarrage
    sudo systemctl enable suricata

    # Vérifier le statut de Suricata
    sudo systemctl status suricata
### C.Installation par les scripts 
Les deux scripts bash (snort_installation.sh & suricata_installation.sh), l'un pour installer Snort et l'autre pour installer Suricata. Ces scripts automatiseront les étapes d'installation et de configuration de base pour les deux outils sur une distribution Linux (comme Ubuntu).
#### Comment utiliser ces scripts ?
    1. Rendez le fichier exécutable 
    
    chmod +x install_snort.sh

    chmod +x install_suricata.sh

    2. Exécutez les scripts 

    sudo ./install_snort.sh
    
    sudo ./install_suricata.sh
Ces scripts automatisent les principales étapes d'installation, mais n'oubliez pas de configurer les fichiers de configuration selon votre réseau et vos besoins (interfaces réseau, chemins de règles, etc.).

