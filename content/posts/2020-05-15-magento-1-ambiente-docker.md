---
title: Magento 1 – ambiente Docker con PHP 5.4
author: olivertar
type: post
date: 2020-05-15T13:47:40+00:00
url: /magento-1-ambiente-docker/
featured_image: /wp-content/uploads/2020/05/mage17.jpg
image: /img/placeholder.jpg
categories:
  - General
  - Magento
  - Productividad
tags:
  - magento

---
Recientemente un antiguo cliente me contacto porque tenia un problema con la integración de Paypal.

En realidad el problema no era Paypal sino que la versión de Magento que estaban usando era 1.7.0.2 y la integración incluida en Magento ya no funcionaba o no lo hacia como el cliente necesitaba.

Es el típico caso donde el sitio funciona mas o menos bien (cuestiones de seguridad al margen) y ha sido personalizado de tal manera que se dificulta, ya no la migración a M2 sino la actualización de M1.

Dejando de lado lo anecdotico de encontrar una versión tan antigua de Magento, el problema que se me presento fue instalar el proyecto en mi entorno local, que en este momento sea el stack de linux o los ambientes dokerizados solo soportan versiones de PHP iguales o mayores de 5.6 y esa versión de Magento necesita PHP 5.4

Buscando un poco encontré una imagen de Docker hecha por [alexcheng](https://hub.docker.com/r/alexcheng/magento/) que permite instalar Magento 1 con propósitos de testing. Esto significa que incluye funcionalidades para instalar una versión especifica de Magento.

Lo que yo necesitaba es solo el entorno para instalar el proyecto del cliente, así que utilice esa imagen como punto de partida.

A continuación el docker compose que use donde ademas de la imagen mencionada como server web/php esta MySQL y PhpMyAdmin:

```dockerfile
version: '3.0'
services:
  web:
    image: alexcheng/magento:1.7.0.2
    ports:
      - "80:80"
    links:
      - db
    env_file:
      - env
    volumes:
        - ./www:/var/www/html
  db:
    image: mysql:5.6.23
    volumes:
      - ./db:/var/lib/mysql/data
    ports:
      - "3306:3306"
    #env_file:
    environment:
      - MYSQL_DATABASE=magento
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_USER=magento
      - MYSQL_PASSWORD=magento
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8580:80"
    links:
      - db
```

El directorio donde deben estar los archivos del proyecto esta determinado en la linea:

&#8211; ./www:/var/www/html

En mi caso el directorio “”www” en el mismo nivel que “docker-compose.yml”

Las variables de entorno (ver la documentación de la imagen) las toma de un archivo “env” en el mismo nivel que “docker-compose.yml” (las variables Magento en realidad no son necesarias para mi propósito

En mi caso contiene:

```
MYSQL_HOST:mysql
MYSQL_DATABASE:magento
MYSQL_USER:magento
MYSQL_PASSWORD:magento
MAGENTO_LOCALE:en_GB
MAGENTO_TIMEZONE:Pacific/Auckland
MAGENTO_DEFAULT_CURRENCY:NZD
MAGENTO_URL:http://local.magento
MAGENTO_ADMIN_FIRSTNAME:Admin
MAGENTO_ADMIN_LASTNAME:MyStore
MAGENTO_ADMIN_EMAIL:admin@example.com
MAGENTO_ADMIN_USERNAME:admin
MAGENTO_ADMIN_PASSWORD:123456
```

La conexción a la Database se configura con estas variables:

```
      - MYSQL_DATABASE=magento
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_USER=magento
      - MYSQL_PASSWORD=magento
```

Hecho esto ya podemos lanzar los contenedores con docker-compose up -d

Y si agregamos local.magento a nuestro archivo “/_etc_hosts” deberíamos poder ver el servidor web en:

<http://local.magento>

y phpmyadmin en:

<http://local.magento:8580>

Comencemos a instalar nuestro proyecto Magento 1

Colocamos o clonamos los archivos de nuestro proyecto en la carpeta “www” y agregamos/ modificamos nuestro archivo “app/etc/local.xml” para establecer la coneccion a la DB usando los valores de las variables “MYSQL_XXXXX” mencionadas mas arriba.

Como “host” se debe usar el alias de la imagen de mysql, en este caso “db”. Si usan “localhost” no va a funcionar.

El próximo paso es restaurar la DB. Como estamos trabajando en un ambiente dokerizado el comando sql para restaurar la DB debemos correrlo dentro del contenedor con “docker exec”

El comando requiere como parámetro el nombre del contenedor de la imagen MySQL. 

Para averiguarlo podemos hacerlo con el comando docker ps que nos muestra todos los contenedores que están corriendo actualmente.

En este caso si el nombre de la carpeta donde esta “docker-compose.yml” es “proyecto”, el nombre del contenedor será “proyecto\_db\_1” (asumiendo que solo tienen una instancia corriendo)

Este es el comando completo, se deben reemplazar las variables por los valores usados en “docker-compose.yml” (MYSQL\_USER, MYSQL\_ROOT\_PASSWORD, MYSQL\_DATABASE)

```bash
$ docker exec -i proyecto_db_1 mysql -u MYSQL_USER -p MYSQL_ROOT_PASSWORD --database= MYSQL_DATABASE &lt; www/dump_proyecto.sql
```

Restaurada la DB y ajustados los parámetros de configuración de Magento (base url, etc) el proyecto debería estar disponible en: http://local.magento/

Es importante recordar que cualquier operación que se necesite hacer sobre Magento por linea de comando debe hacerse desde dentro del contenedor.

Para acceder al shell podemos hacer :

```bash
$ docker exec -it proyecto_web_1 /bin/bash
```

Donde “proyecto\_web\_1” es el nombre de la instancia del contenedor “web” (docker ps)

Listo! Ya estamos en condiciones de trabajar en nuestro proyecto Magento 1
