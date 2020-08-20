# Lancement d'une instance qui fait tourner Apache2, PHP et Wordpress

On crée tout d'abord une instance grâce à la console AWS que l'on utilisera avec la même paire de clé keyfirst.pem.

## Se connecter sur l'instance en SSH
~~~~
ubuntu@ip-172-31-40-221:sudo apt update && sudo apt upgrade -y
ubuntu@ip-172-31-40-221:sudo apt install apache2 -y
ubuntu@ip-172-31-40-221:sudo apt install php -y
ubuntu@ip-172-31-40-221:sudo apt install php7.2-bz2 php7.2-zip php7.2-xml php7.2-curl php7.2-bz2 php7.2-zip php7.2-xml php7.2-curl php-mysql
ubuntu@ip-172-31-40-221:cd /tmp
ubuntu@ip-172-31-40-221:/tmp$ wget https://wordpress.org/latest.tar.gz
ubuntu@ip-172-31-40-221:/tmp$ tar -zxvf latest.tar.gz
ubuntu@ip-172-31-40-221:/tmp$ sudo mv wordpress /var/www/html/
ubuntu@ip-172-31-40-221:/tmp$ sudo chown www-data.www-data /var/www/html/wordpress/* -R
ubuntu@ip-172-31-40-221:/tmp$ cd /var/www/html/wordpress
ubuntu@ip-172-31-40-221:/var/www/html/wordpress$ mv wp-config-sample.php wp-config.php
~~~~

## Puis on modifie la configuration de Wordpress pour qu'il utilise la base de donnée que l'on va créer
~~~~
ubuntu@ip-172-31-40-221:/var/www/html/wordpress$ vi wp-config.php

/** The name of the database for WordPress */
define( 'DB_NAME', 'wp_db' );

/** MySQL database username */
define( 'DB_USER', 'thomas' );

/** MySQL database password */
define( 'DB_PASSWORD', 'thomas' );

/** MySQL hostname */
define( 'DB_HOST', '172.31.58.165:3306' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
~~~~

# Lancement d'une instance qui fait tourner MariaDB

## On installe MariaDB
ubuntu@ip-172-31-58-165:sudo apt update && sudo apt upgrade -y
ubuntu@ip-172-31-58-165:sudo apt install mariadb-server -y

## Puis on passe à la configuration de la base de donnée et de l'insertion des données nécessaires à Wordpress
ubuntu@ip-172-31-58-165:sudo mysql_secure_installation
ubuntu@ip-172-31-58-165:sudo mysql
CREATE DATABASE wp_db CHARACTER SET UTF8 COLLATE UTF8_BIN;
CREATE USER 'thomas'@'%' IDENTIFIED BY 'thomas';

ubuntu@ip-172-31-58-165:sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf 
bind-address                              = 0.0.0.0

# Pour que les VM puissent communiquées entre elles

On ajoute 1 règle pour les 2 instances sur tout les ports et pour tout type de connection en renseignant l'IP de l'autre instance.
Puis pour accéder au site, on ajoute aussi une règle HTTP - TCP - Port 80 - Source 0.0.0.0/0 pour autoriser les requêtes HTTP et pouvoir accéder au site comme sur le TP1.