# Docker-compose with Lamp + WordPress
[![Build](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

---

## Description
It's just a sample compose file with WordPress. So, maybe WordPress developers who you guys need to install a WordPress container in one click. Also, it worked with a container so you need to install any other packages with your machine

----
## Feature
- Easy to install WordPress and Nginx with Docker containers (MySQL, WordPress, Nginx)
- Easy to build lamp, WordPress containers and network, volume in a single click

-----

## Includes

3 Containers for LAMP (MySQL, WordPress+PHP-FPM, Nginx)
2 Volumes (MySQL, WordPress)
1 Network (For Container interconnecting)

----
## Pre-Requests
- Need to install docker and docker-compose
- You have a basic knowledge of what is docker.

-----

## How to install docker, docker-compose.
### Docker installation 

Please refer the doc for [docker installation](https://docs.docker.com/engine/install)

```sh
yum install docker -y
yum install git -y
```
### docker-compose installation
Please refer the doc for [docker-compose installation](https://docs.docker.com/compose/install/)
```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
docker-compose version   
```

> you guys can use [play with Docker](https://labs.play-with-docker.com/) which includes pre-installed docker and compose. It looks like a terminal and it's built from alpine OS. So, it's just used for learning purpose so who you guys can use 4hrs session without any additional installation.
----

## How to use the docker-compose file? and lamp, WordPress installation.

Please note that the docker installation was done before this. 

```sh
git clone https://github.com/yousafkhamza/docker-compose-lamp-wordpress.git
cd docker-compose-lamp-wordpress
docker-compose up -d               <----------- used for up 3 containers and docker network, volume (-d it's running a detached mode)
```
_please refer to this docker commands which you can see the installed container, network, volume details_
```sh
docker-compose ps                  <---------------- Please note that this command only works with the installation directory
docker network ls                  <---------------- Network list which you used
docker volume ls                   <---------------- Volume list which you used.
docker-compose down -v             <---------------- if you need to remove all containers and volumes using these commands (Please note that this command only works with the installation directory)
```
_sample-screenshots_
> Git Cloning
![alt text](https://i.ibb.co/0VYQC8z/Screenshot-2.png)

> Compose build the complete structure with lamp and wordpress.
![alt text](https://i.ibb.co/yVCxdm6/Screenshot-3.png)
![alt text](https://i.ibb.co/7gSm9yC/Screenshot-4.png)
![alt text](https://i.ibb.co/TLMGPVF/Screenshot-5.png)

> List buileded containers and dependencies.
![alt text](https://i.ibb.co/RhqdWMD/Screenshot-7.png)

> Uninstall all the containers and dependancies. 
![alt text](https://i.ibb.co/CzghSG9/Screenshot-8.png)

----
## Behind the file.
vim docker-compose.yml
```sh
version: '3'
services:
  database:                                    #<---- Database container
    image: mysql:5.7
    container_name: mysql
    restart: always
    networks:
      - wp-net
    volumes:
      - mysql-volume:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: mysqlroot123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:                                    #<---- WordPress Container with PHP-FPM
    image: wordpress:php7.4-fpm-alpine
    container_name: wordpress
    restart: always
    depends_on:
      - database
    networks:
      - wp-net
    volumes:
      - wp-volume:/var/www/html
    environment:
      WORDPRESS_DB_HOST: database
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress 
      WORDPRESS_DB_NAME: wordpress


  nginx:                                    #<---- Nginx Container
    image: nginx:alpine
    container_name: nginx
    restart: always
    ports:
      - "80:80"
    volumes:
      - wp-volume:/var/www/html
      - ./nginx.conf:/etc/nginx/conf.d/default.conf     #<---- Configuration file
    networks:
      - wp-net

networks:                                    #<---- Networks used 
  wp-net:
volumes:                                    #<---- Volumes used
  mysql-volume:
  wp-volume:
```
vim nginx.conf
```sh
server {
  listen 80;
  listen [::]:80;
  access_log off;
  root /var/www/html;
  index index.php;
  server_name example.com;        #<--------  your domain_name
  server_tokens off;
  location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ /index.php?$args;
  }
  # pass the PHP scripts to FastCGI server listening on wordpress:9000
  location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass wordpress:9000;        #<-------- PHP-FPM Nginx container connect with container name 
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
  }
}
```

----

#### Additional 

 I also uploaded the same with self-signed SSL so who you guys used that one please enter that directory and create your domain SSL under that working directory. So, please find the below command and try the same.
 
 ```sh
 cd with-self-signed-ssl/
 openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout example.com.key -out example.com.crt
 ```
 > Also, with self-signed SSL config and compose file have slight differences so please look at the same from your point of view. Please let me know if you have any doubts regarding this blog then I will help you to resolve the same.
 
----

### Docker-Compose Command Cheat sheet
![alt text](https://i.ibb.co/D7LHWMx/docker-compose-cheat-sheet-ryan-prater.png)

----
## Conclusion

Its a simple to install a lamp stack with WordPress maybe it's useful for WordPress developers.


### ⚙️ Connect with Me

<p align="center">
<a href="mailto:yousaf.k.hamza@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/yousafkhamza"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.instagram.com/yousafkhamza"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://wa.me/%2B917736720639?text=This%20message%20from%20GitHub."><img src="https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/></a>


