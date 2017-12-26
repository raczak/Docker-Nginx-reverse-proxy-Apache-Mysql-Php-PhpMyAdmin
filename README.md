[![GitHub license](https://img.shields.io/badge/license-Apache-green.svg)](http://git.kaido.ovh/Kido/Deskofus/raw/master/license) [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)
# Sosnoob - Créez votre serveur Nginx (en proxy), apache, php, mysql et phpmyadmin entièrement avec `docker` !

Dans le but de répondre à un besoin exprimé par la communauté, j'ai créé une stack serveur-web `full docker` dispo pour : </br>
- `Windows`
- `OSX` 
- `Linux`

**Tuto de mise en place complet** : [Si vous souhaitez vous former](https://www.sosnoob.com/nginx-apache-php-mysql-avec-docker/)

Avec cette architecture, je vous propose de mettre en place `Nginx` en serveur proxy pour rediriger nos requêtes, `apache` en serveur web avec `php`, `mysql` pour la gestion des données de nos apps et `phpmyadmin` pour gérer la base de données.

## Installation
* Clonez ce dépôt à l'emplacement de votre choix sur votre serveur.
* Modifiez le fichier `.env` avec les informations de votre serveur et ce que vous désirez pour mot de passe MySQL (néanmoins cela fonctionne avec les paramètres de bases).
* Exécutez la commande suivante : `docker-compose up -d`
* Faites un `docker ps` et vous constaterez le bon fonctionnement de vos conteneurs.

## Configuration
### Nginx
Les fichiers de configuration de `Nginx` sont dans le dossier `etc/nginx/`. Vous trouverez dedans tous les fichiers de conf utilisés par le container docker.
Ne modifiez que le fichier `etc/nginx/conf.d/default.template` car les modifications seront automatiquement effectuées sur `default.conf`.

### Apache
Les fichiers de configuration de `Apache` sont dans le dossier `etc/apache/`. Vous trouverez dedans tous les fichiers de conf utilisés par le container docker dont les dossiers `sites-available` et `sites-enabled`.

Le dossier contenant vos sites web se trouve dans `web/`. Pour chaque site, créé un sous-dossier www.monsite.com et éditez les fichiers de conf d'apache, ainsi que ceux d'Nginx.

### Mysql

### Variables d'environement
Le fichier `.env`, vous permettra de définir l'adresse de votre serveur, le nom de compte mot de passe Mysql et d'autres options de constructions de containers.

### [Optionel] Sécurisez votre site via SSL avec letsencrypt
Comming soon !

Un bug ? Des questions ? Vous voulez rejoindre la communauté Sosnoob.com ? [**Notre discord**](http://)

## Documentation
[Accéder au tuto pour comprendre le projet et pouvoir y contribuer](https://www.sosnoob.com/nginx-apache-php-mysql-avec-docker/)
