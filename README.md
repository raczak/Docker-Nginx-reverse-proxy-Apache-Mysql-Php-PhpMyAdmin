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
* Exécutez la commande suivante : `docker build -t apachemy`.
* Puis exécutez la commande suivante : `docker-compose up -d`.
* Faites un `docker ps` et vous constaterez le bon fonctionnement de vos conteneurs.

## Configuration
**:warning: Avant de déclarer un bug, vérifiez que vos ports ne sont pas déjà utilisés par d'autres services**

### Ports utilisés par la stack
| Port       | Service        |
| ------------- |:-------------|
| `80`     | Nginx  |
| `443`    | Nginx |
| `8099` | Apache |
| `8080` | Phpmyadmin |
| `8989` | MySQL |
### Nginx
Les fichiers de configuration de `Nginx` sont dans le dossier `etc/nginx/`. Vous trouverez dedans tous les fichiers de conf utilisés par le container docker.
Ne modifiez que le fichier `etc/nginx/conf.d/default.template` car les modifications seront automatiquement effectuées sur `default.conf`.

**Après l'installation faites :**
1. Editez le fichier `default.template`et remplacez à la ligne `server_name` par l'URL de votre site.
2. Redémarrez le container Nginx avec `sudo docker restart nginx-docker`.

### Apache
Les fichiers de configuration de `Apache` sont dans le dossier `etc/apache/`. Vous trouverez dedans tous les fichiers de conf utilisés par le container docker dont les dossiers `sites-available` et `sites-enabled`.

**Après l'installation faites :**
1. Renommer le fichier `etc/apache/site-available/toto.com.conf` par celui de votre site.
2. Remplacer de-dans les liens par ceux de votre site (ne changez pas le lien du **DocumentRoot** ni **Directory**, seulement le `toto.com`).
3. Exécutez la commande `sudo docker exec -t apache-docker a2ensite toto.com.conf` dans ce même dossier.

Le dossier contenant vos sites web se trouve dans `web/`. Pour chaque site, créé un sous-dossier www.monsite.com et éditez les fichiers de conf d'apache, ainsi que ceux d'Nginx.

### Mysql
Pour ne pas perdre nos données à chaque redémarrage de docker, nous allons les stocker (shared folder) dans notre machine dans : `data/db/mysql`.

### Variables d'environement
Le fichier `.env`, vous permettra de définir l'adresse de votre serveur, le nom de compte mot de passe Mysql et d'autres options de constructions de containers.

### [Optionnel] Sécurisez votre site via SSL avec letsencrypt
1. Aller dans le dossier `etc/ssl/`.
2. Exécutez la commande `docker-compose up -d`.
3. Exécutez la commande :
```
$ sudo docker run -it --rm -p 443:443 -p 83:80 --name certbot \
-v "/link_to/your_folder/docker-nginx-php-mysql/etc/ssl/mycertificate/letsencrypt:/etc/letsencrypt" \
-v "/link_to/your_folder/docker-nginx-php-mysql/etc/ssl/mycertificate/letsencrypt-lib/:/var/lib/letsencrypt" \
```
Répondez aux questions du **certbot** qui vous générera les certificats nécessaires dans `/etc/ssl/mycertificate/letsencrypt/live/toto.com/`

4. Editez le fichier `etc/nginx/conf.d/default.template` et dé-commentez la partie `To use https`.
5. Remplacez à la ligne `server_name` le nom du site par l'URL de votre site.
6. Remplacer aux lignes `ssl_certificate` et `ssl_certificate_key` les nom des certificats par les vôtres (créés à l'étape 3).
7. Redémarrez le container Nginx avec `sudo docker restart nginx-docker`.

Félicitations vous avez mis en place votre site en HTTPS !
Pour le renouveler, rien de plus simple, exécutez de nouveau l'étape 3.
![](https://raw.githubusercontent.com/raczak/Docker-Nginx-reverse-proxy-Apache-Mysql-Php-PhpMyAdmin/master/https.PNG)

## [Optionnel] Rediriger les requêtes sur le port 80 (http) vers le 443 (https)
Pour vous assurer de ne plus avoir d'accès non sécurisés :
1. Editez le fichier `etc/nginx/conf.d/default.template` et dé-commentez la partie `To forward http to https`.
2. Remplacez à la ligne `server_name` l'url par celle de votre site.
3. Redémarrez le container Nginx avec `sudo docker restart nginx-docker`.

Un bug ? Des questions ? Vous voulez rejoindre la communauté Sosnoob.com ? [**Notre discord**](http://)

## Documentation
[Accéder au tuto pour comprendre le projet et pouvoir y contribuer](https://www.sosnoob.com/nginx-apache-php-mysql-avec-docker/)
