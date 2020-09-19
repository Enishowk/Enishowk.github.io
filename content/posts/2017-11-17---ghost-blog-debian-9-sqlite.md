---
title: "Ghost, Debian 9, Sqlite, Apache, Node 8.9, SSL"
date: "2017-11-17"
template: "post"
draft: false
slug: "ghost-blog-debian-9-sqlite"
category: "Dev"
tags:
  - "Dev"
  - "Ghost"
  - "Debian"
  - "Sqlite"
  - "Apache"
  - "Node"
  - "SSL"
description: "Le moteur de blog de Ghost est assez bien foutu, tourne avec NodeJS et permet une customisation assez poussée. Cependant, son installation peut s'avérer problématique lorsque l'on s'écarte des recommandations."
# socialImage: "/media/image-0.jpg"
---

Ghost recommande :
* Ubuntu 16.04
* MySQL
* NGINX (minimum of 1.9.5 for SSL)
* Node v6 installed via NodeSource

Bien sûr, mon serveur tourne sous Debian 9, Apache, Node 8.9 et je voulais une base de données Sqlite. 

Après avoir eu quelques problèmes pendant l'installation, je voulais faire un petit post explicatif à ce sujet.

# Installation d'Apache, NodeJS

Je pars du principe que vous avez déjà Debian 9 à jour, Apache et Node d'installés. Mais si besoin, voici les commandes pour les installer.

`$ apt-get install apache2`  

`$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`  
`$ sudo apt-get install -y nodejs`  


# Ajout d'un utilisateur
On va tout d'abord commencer par créer un nouvel utilisateur, évitant ainsi de lancer notre serveur node en tant que root. Ghost précise :
> N'appelez pas votre utilisateur **ghost**, l'outil Ghost-CLI va créer un utilisateur **ghost** pour administrer le blog. L'utilisateur que vous créez va être utilisé pour administrer le serveur.

Il suffit donc de lui trouver un joli nom et de le créer avec cette commande :  
`$ adduser <votreUser>`  

L'utilisateur fraîchement créé a besoin des privilèges Super Utilisateur plus tard, pour cela :  
`$ usermod -aG sudo <votreUser>`  

On peut maintenant se logger avec :  
`$ su - <votreUser>`  

# Installation de Ghost

Ghost fournit un outil pour l'administration serveur de son moteur de blog. Il permet d'installer, de mettre à jour, de supprimer, ou même de démarrer son blog et de lister les blogs si vous en avez plusieurs.
La commande pour récupérer cet outil :  
`$ sudo npm i -g ghost-cli`  

Maintenant que `ghost-cli` est disponible, il faut choisir le dossier d'installation, en être le propriétaire (avec le nouvel utilisateur), et s'y rendre.  
`$ sudo mkdir -p /var/www/ghost`  
`$ sudo chown [votreUser]:[votreUser] /var/www/ghost`  
`$ cd /var/www/ghost`  

Et c'est ici que cela se complique (pas beaucoup je vous rassure).
Ghost est fait pour fonctionner avec une base de données MySQL. Il faut donc, pour utiliser une base de données Sqlite, ajouter le paramètre **local** à la commande d'install.

`$ ghost install local`

Cela va installer le blog dans le dossier courant. 

> Il est fort probable que vous ayez une erreur sqlite client. Pour cela, il faut désinstaller `ghost-cli` et le reinstaller... Et si vous avez l'erreur npm qui va avec, voici donc la commande miracle, trouvée sur leur Github.

`$ sudo npm un -g ghost-cli && sudo npm i --unsafe-perm -g ghost-cli`

Vous pouvez ensuite relancer la commande d'install si besoin, tout devrait fonctionner.

*Quelques commandes utiles :*  
* Démarrer le blog : `$ ghost start`  
* Lister les blogs : `$ ghost ls`  
* Arrêter le blog : `$ ghost stop`  

# Apache - VHost
Une fois l'installation effectuée, il faut s'occuper de la partie Apache.
Pour cela, rien de plus simple, il suffit d'ajouter un VHost dans le dossier **sites-available** d'Apache.
`$ sudo vim /etc/apache2/sites-available/ghostblog.conf`

Voici un exemple de configuration à ajouter dans le fichier de conf.

```
<VirtualHost *:80>
    #Domain Name
    ServerName votredomaine.fr
    ServerAlias www.votredomaine.fr

    #HTTP proxy/gateway server
    ProxyRequests off 
    ProxyPass / http://127.0.0.1:2368/ 
    ProxyPassReverse / http:/127.0.0.1:2368/     
</VirtualHost>
```

Activez ensuite les modules `proxy` et `proxy_http`.  
`$ sudo a2enmod proxy proxy_http`  

Activez maintenant le site.  
`$ sudo a2ensite ghostblog`  

Il ne reste plus qu'à redémarrer Apache et Ghost.  
`$ sudo systemctl restart apache2`  
`$ sudo systemctl restart ghost`  

# Custom domain

Pour utiliser votre propre domaine, une configuration est nécessaire au niveau de votre fournisseur de domaine.
Si vous êtes chez OVH, il faut vous rendre sur votre espace client, aller dans l'administration de votre domaine et cliquer sur l'onglet **Zone DNS**.
Ici, cliquez sur **Ajouter une entrée** et choisissez le champ DNS de type A.
Rentrez l'adresse IP de votre serveur dans le champ **Cible** et validez.

![dns1](/media/dns1.jpg)

La mise à jour des DNS n'est pas instantanée. Attendez quelques heures pour que votre nom de domaine pointe bien vers votre serveur.

# SSL
Pour finir, voici comment mettre en place un certificat TLS/SSL sur le serveur Apache en quelques minutes.

Pour cela, nous allons utiliser l'outil `cerbot`.  

L'installation se fait à travers cette commande :  
`$ sudo apt-get install python-certbot-apache`

Ensuite, exécutez la commande  
`$ sudo certbot --apache`

**Certbot** récupère un certificat et met à jour la configuration des Vhost d'Apache qu'il trouve.

![SSL1](/media/SSL1.jpg)

Sélectionnez donc vos domaines et appuyez sur entrer.

Les certificats proposés par Certbot (Let's Encrypt) ne sont valables que 90 jours, il faut donc les renouveler. Pour cela, on peut utiliser la commande `certbot renew` et créer une tâche récurrente qui exécutera cette commande. 
Lancez la commande :  
`$ sudo crontab -e`  
Et ajoutez cette ligne :  
`0 4 * * 7 certbot renew`  

Les certificats seront ainsi mis à jour chaque semaine, le dimanche à 4h du matin.

Voilà, votre blog est maintenant configuré sur votre serveur. N'hésitez pas à venir me parler sur Twitter si vous avez des questions ou suggestions. 

