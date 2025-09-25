# Jour 1 : Introduction et Docker de base

Objectif : Comprendre les concepts de Docker et lancer ses premiers conteneurs.

Contenu :

Concepts : images, conteneurs, Dockerfile

<h2> Installation et vérification : </h2> 

     docker --version
     docker info
     docker run hello-world
     
<h2>Commandes de base pour conteneurs :</h2>

     docker ps
     docker ps -a
     docker stop <nom|id>
     docker start <nom|id>
     docker rm <nom|id>

<h2> Création d’un conteneur Nginx simple : </h2>

    docker run -d -p 8080:80 --name mon-nginx nginx
    
# Jour 2 : Docker Volumes et Apache

Objectif : Apprendre la persistance des données avec les volumes et lancer un serveur Apache.

Contenu :

<h2>Docker Volumes :</h2>

     docker volume create monvolume
     docker volume ls
     docker run -v monvolume:/chemin/conteneur ...
     docker volume rm monvolume

<h2>Conteneur Nginx avec volume :</h2> 

     docker volume create nginx-content
     docker run -d -p 8080:80 -v nginx-content:/usr/share/nginx/html --name mon-nginx nginx

<h2>Conteneur Apache avec volume :</h2>

     docker volume create apache-content
     docker run -d -p 8081:80 -v apache-content:/usr/local/apache2/htdocs --name mon-apache httpd:2.4

# Jour 3 : MySQL et Docker Compose     

Objectif : Gérer des conteneurs de base de données et orchestrer plusieurs conteneurs avec Docker Compose.
Contenu :
<h2> Conteneur MySQL avec volume :></h2> 

     docker volume create mysql-data
     docker run -d -e MYSQL_ROOT_PASSWORD=rootpassword -v mysql-data:/var/lib/mysql --name mon-mysql mysql:5.7

<h2>Docker Compose multi-conteneurs (Nginx + Apache + MySQL) :</h2>


     version: '3'
     services:
       web-nginx:
         image: nginx
         ports:
           - "8080:80"
         volumes:
           - nginx-content:/usr/share/nginx/html
       web-apache:
         image: httpd:2.4
         ports:
           - "8081:80"
         volumes:
           - apache-content:/usr/local/apache2/htdocs
       db:
         image: mysql:5.7
         environment:
           MYSQL_ROOT_PASSWORD: rootpassword
         volumes:
           - mysql-data:/var/lib/mysql
     volumes:
       nginx-content:
       apache-content:
       mysql-data:

