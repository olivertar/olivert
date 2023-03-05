---
title: Shopware 6 – Instalación parte 2 – Plantilla Development
author: olivertar
type: post
date: 2021-05-14T18:03:00+00:00
url: /shopware-6-instalacion-plantilla-development/
image: /img/placeholder.jpg
categories:
  - General
  - Shopware
tags:
  - shopware
  - shopware 6

---
En un post [anterior](https://olivert.com.ar/shopware-6-instalacion-parte-1-plantillas) expliqué que _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ tiene 2 plantillas distintas para la instalación. 

En este post voy a explicar como usar el “**template**” &#8220;**Development**&#8220;.

En este caso voy a explicar como instalar _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ usando el ambiente dockerizado que incluye el &#8220;**template**&#8221; &#8220;**Development**&#8220;.

Es requisito que la máquina local donde instalaremos _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ tenga:

  * PHP 7.4+ CLI
  * docker
  * docker-compose
  * bash

También debemos asegurarnos que el puerto 80 de la máquina no esté ocupado y si lo está debemos detener la aplicación que lo está usando.

El primer paso es clonar el templete en algún lugar de nuestro disco

```bash
$ git clone https://github.com/shopware/development.git testshopware
```

Entramos al directorio donde se realizó el clon

```bash
$ cd testshopware
```

Levantamos los contenedores docker

```bash
$ ./psh.phar docker:start
```

Si todo salio bien la terminal tendrá una salida como esta.

```bash
SHOPWARE Developer Version
       _
      | |
   ___| |__   ___  _ ____      ____ _ _ __ ___
  / __| '_ \ / _ \| '_ \ \ /\ / / _` | '__/ _ \
  \__ \ | | | (_) | |_) \ V  V / (_| | | |  __/
  |___/_| |_|\___/| .__/ \_/\_/ \__,_|_|  \___|
                  | |
                  |_|

Using .psh.yaml.dist


Starting Execution of 'docker:start' ('/home/xxxxx/www/testshopware/dev-ops/docker/actions/start.sh')


(1/3) Starting                                                                                                                                                                                              
> dev-ops/docker/scripts/check_permissions.sh

(2/3) Starting                                                                                                                                                                                              
> if [ -n "" ]; then docker-sync start && echo "\n docker-sync is initially indexing files. It may take some minutes, until code changes take effect"; fi

(3/3) Starting                                                                                                                                                                                              
> docker-compose build --parallel && docker-compose up -d
        cypress uses an image, skipping

        Step 1/14 : ARG IMAGE_PHP=webdevops/php-apache


sl.google.com/linux/linux_signing_key.pub | apt-key add -     && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'         && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -     && sh -c 'echo "deb [arch=amd64] https://download.docker.com/linux/debian stretch stable" >> /etc/apt/sources.list.d/docker.list'         && mkdir -p /usr/share/man/man1     && curl -sL https://deb.nodesource.com/setup_12.x | bash         && mkdir -p ${NPM_CONFIG_CACHE}     && apt-install default-mysql-client nodejs google-chrome-stable libicu-dev graphviz vim gnupg2 docker-ce=5:18.09.7~3-0~debian-stretch libgtk2.0-0 libgtk-3-0 libgbm-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 libxtst6 xauth xvfb         && npm install -g npm@^6.14.11     && npm i forever -g     && chown -R ${USER_ID}:${GROUP_ID} ${NPM_CONFIG_CACHE}         && ln -s /app/psh.phar /bin/psh         && pecl install pcov     && docker-php-ext-enable pcovSuccessfully built cf174534f1bd                                                                                                                                                                                  

         ---> e5bb15b8747c
        Step 13/14 : COPY php-config.ini /usr/local/etc/php/conf.d/99-docker.ini
         ---> Using cache
         ---> 1de071f8dc86
        Step 14/14 : WORKDIR /app
         ---> Using cache
         ---> 5127dd0825c6

Building app_mysql  ... done
        Successfully tagged testshopware_app_mysql:latest
Building app_server ... done
Creating network "testshopware_shopware" with the default driver
Creating testshopware_adminer_1       ... done
Creating testshopware_app_server_1    ... done
Creating testshopware_app_mysql_1     ... done
Creating testshopware_mailhog_1       ... done
Creating testshopware_cypress_1       ... done
Creating testshopware_elasticsearch_1 ... done

Duration: 4s
All commands successfully executed!
```

Si miramos detenidamente la salida de la consola encontraremos mucha información interesante sobre el entorno que levantamos.

Luego, accedemos al shell del contenedor de la app

```bash
$ ./psh.phar docker:ssh
```

Podremos ver el prompt del contenedor al que accedimos: /app$ 

```bash
###################
SHOPWARE Developer Version

       _
      | |
   ___| |__   ___  _ ____      ____ _ _ __ ___
  / __| '_ \ / _ \| '_ \ \ /\ / / _` | '__/ _ \
  \__ \ | | | (_) | |_) \ V  V / (_| | | |  __/
  |___/_| |_|\___/| .__/ \_/\_/ \__,_|_|  \___|
                  | |
                  |_|

Using .psh.yaml.dist


Starting Execution of 'docker:ssh' ('/home/oliver/www/testshopware/dev-ops/docker/actions/ssh.sh')


(1/1) Starting
> docker exec -i --env COLUMNS=`tput cols` --env LINES=`tput lines` -u 1000:1000 -t f2c501cad2829974a7523875c6a302faa4e0d33e7a108694929c8474ffffcf26 bash
application@f2c501cad282:/app$
```

Finalmente ejecutamos el instalador

```bash
$ ./psh.phar install
```

El instalador descargara las dependencias, instalara la plataforma, creara la base de datos y configurara la aplicación.

En la versión que yo estoy probando fueron 42 instrucciones. Una vez mas leer la salida de la terminal nos va a aportar mucha información sobre la plataforma.

Cuando finalmente veamos el mensaje &#8220;**All commands successfully executed!**&#8221; podremos ir a nuestro navegador e ingrear la url donde podemos ver el resultado

```bash
$ http://localhost:8000/
```

El usuario y password del administrador son: admin y shopware y la url del backend es:

```bash
$ http://localhost:8000/admin
```

Por ultimo si queremos salir del container docker de la app hacemos:

```bash
$ exit
```

Y si queremos detener todo el entorno doquerizado, desde fuera del container de la app:

```bash
$ ./psh.phar docker:stop
```

La próxima vez que queramos usar la versión de Shopware 6 que hemos instalado en nuestra maquina local simplemente tenemos que iniciar el entono docker

```bash
$ ./psh.phar docker:start
```

Con estos simples pasos y sin ningún setup especial de nuestra maquina local podremos tanto hacer desarrollo de extensiones, contribuir al core de Shopware o simplemente conocer la plataforma.

En el [próximo post](https://olivert.com.ar/shopware-6-instalacion-plantilla-production/) voy a explicar como instalar _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ con la plantilla &#8220;**Production**&#8221; que nos servirá para iniciar un proyecto desde cero.

