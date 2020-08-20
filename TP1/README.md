# Installer un serveur Web public sur l'instance

On crée tout d'abord une instance grâce à la console AWS (avec sa paire de clé pour autoriser la connection SSH).

## Se connecter sur l'instance en SSH
~~~~
$ ssh -i Bureau/firstkey.pem ubuntu@3.85.207.6
~~~~

## Installation d'un serveur web
~~~~
ubuntu@ip-172-31-40-221:~$ sudo apt -y update && sudo -y apt upgrade
ubuntu@ip-172-31-40-221:~$ sudo apt -y install nginx
~~~~

## Vérifier le fonctionnement du site Web
~~~~
ubuntu@ip-172-31-40-221:~$ sudo wget -O - http://localhost/
ubuntu@ip-172-31-40-221:~$ sudo apt -y install w3m
ubuntu@ip-172-31-40-221:~$ sudo w3m http://localhost/
~~~~

## Ajouter une règle entrante dans le groupe de sécurité
<pre>
Sur la console AWS :
Aller dans Groupes de sécurité et modifier les règles entrantes pour autoriser les connections
Ajouter le type HTTP - TCP - Port 80 - Source 0.0.0.0/0
On peut maintenant atteindre le serveur web installé sur la VM à l'adresse correspondante au DNS public (IPv4) fourni par AWS :
<a href="http://ec2-3-85-207-6.compute-1.amazonaws.com">Mon serveur web</a>
</pre> 