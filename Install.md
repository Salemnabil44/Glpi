
## Installation des pré-requis

Faisons une mise-à-jour : sudo apt-get update && sudo apt-get upgrade

Installation d'Apache : sudo apt-get install apache2 -y

Activation d'Apache au démarrage de la machine : sudo systemctl enable apache2

Installation de la base de donnée. Ici nous installons MariaDB : sudo apt-get install mariadb-server -y



Installation des modules annexes :
sudo apt-get install php libapache2-mod-php -y
sudo apt-get install php-{ldap,imap,apcu,xmlrpc,curl,common,gd,json,mbstring,mysql,xml,intl,zip,bz2}

## Configuration de MariaDB

La commande ci-dessous va lancer le processus d'initialisation de la base de données :

1sudo mysql_secure_installation
Répondre Y à toutes les questions qui seront posées pendant l'initialisation.

Suite à la question Change the root password? il faudra entrer le mot de passe de la base de données.

Connection à la base de données : mysql -u root -p

On va configurer ceci :

Un nom de base de données : glpidb
un compte avec des droits d'accès élevé : glpi (il faudra choisir un mot de passe)

Cela va se faire avec les commandes ci-dessous :

create database glpidb character set utf8 collate utf8_bin;
grant all privileges on glpidb.* to glpi@localhost identified by 'motDePasse';
flush privileges;
exit

## Récupération des sources Glpi

On récupère la source : wget https://github.com/glpi-project/glpi/releases/download/10.0.2/glpi-10.0.16.tgz (attention d'avoir la derniere version)

On va mettre le contenu téléchargé dans un autre emplacement : 

sudo mkdir /var/www/html/glpi.monNomDeDomaine
sudo tar -xzvf glpi-10.0.16.tgz
sudo cp -R glpi/* /var/www/html/glpi.monNomDeDomaine/

On va modifier les droits :

sudo chown -R www-data:www-data /var/www/html/glpi.monNomDeDomaine/
sudo chmod -R 775 /var/www/html/glpi.monNomDeDomaine/

## Configuration de PHP

On va tout d'abord éditer le fichier php.ini qui est sous /etc/php/8.1/apache2/
Ensuite on va modifier les paramètres suivants :

memory_limit = 64M
file_uploads = on
max_execution_time = 600
session.auto_start = 0
session.use_trans_sid = 0

Fermer le fichier et redémarrer la machine avec la commande sudo reboot

## Installation de Glpi

On va faire l'installation à partir d'un navigateur web, donc à partir d'une autre machine.
Il va falloir configurer la carte réseau de ta VM, mettre une adresse IP fixe, et que tu ais une autre VM dans le même réseau que celle-ci.

Configuration réseau de la VM Glpi :
Tout d'abord, arrête ta VM.
Dans ton hyperviseur va modifier la carte réseau pour qu'elle ne soit plus en bridge mais en réseau interne.
Redémarre ta VM.
Mettre une adresse IP fixe :
Choisi une adresse IP avec le masque associé et mets-là dans la configuration de ta machine.
Démarre ton autre VM (client Windows ou Linux)
Ping ta VM Ubuntu server pour vérifier que la configuration réseau est bonne.

Si tout est correct,ouvre un navigateur web et entre l'adresse http://adresseIPduserveur/glpi.monNomDeDomaine
Dans les fenêtres graphique de configuration :

Mettre Français en langue
Accepter la licence GPL en cochant la case J'ai lu et accepté...
Cliquer sur installer
Si tout est ok sur la fenêtre cliquer sur continuer

Configuration de la base de données MariaDB :
serveur SQL : 127.0.0.1
utilisateur : glpi
Mot de passe : Mot de passe défini pendant la configuration de la base de données pour le compte glpi

<img width="780" alt="Capture d’écran 2024-07-18 à 16 25 27" src="https://github.com/user-attachments/assets/ad691c61-7050-4854-9477-3bf8199d36bc">


<img width="795" alt="Capture d’écran 2024-07-18 à 16 26 14" src="https://github.com/user-attachments/assets/4d61d616-0b6b-42f1-832e-f7165b101464">


<img width="770" alt="Capture d’écran 2024-07-18 à 16 26 36" src="https://github.com/user-attachments/assets/e9e8041b-f80a-4687-be22-9c7aa506fc38">


<img width="778" alt="Capture d’écran 2024-07-18 à 16 26 54" src="https://github.com/user-attachments/assets/c169d8b5-a768-47d2-87f0-07140ee4c18e">


<img width="1441" alt="Capture d’écran 2024-07-18 à 16 28 33" src="https://github.com/user-attachments/assets/0ea563eb-a83f-4072-85c0-05a45e4e4f16">






