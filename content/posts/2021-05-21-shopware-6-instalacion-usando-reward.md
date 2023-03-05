---
title: Shopware 6 – Instalación usando Reward
author: olivertar
type: post
date: 2021-05-21T17:54:00+00:00
url: /shopware-6-instalacion-usando-reward/
image: /img/placeholder.jpg
categories:
  - General
  - Shopware
tags:
  - reward
  - shopware
  - shopware 6

---
Este post documenta como instalar [Shopware 6](href="https://www.shopware.com) con el orquestador de docker _[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_.  


_[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_ esta inspirado en [Warden](href="https://docs.warden.dev/) pero me parece un poco mas flexible para lo que suelo necesitar en mi estación local para el desarrollo con PHP, Magento 2, Shopware o WordPress.  

Si estas interesado en conocer mas sobre _[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_ o [como instalarlo](https://rewardenv.readthedocs.io/en/latest/installation.html) la documentación es bastante clara.

Para crear un host usando _<a rel="noreferrer noopener" aria-label="Reward (opens in a new tab)" href="https://rewardenv.readthedocs.io/en/latest/index.html" target="_blank">Reward</a>_ es imprescindible haberlo levantado, si no lo tenemos corriendo lo hacemos con:

```bash
$ reward svc up
```

Creamos el directorio para el proyecto y entramos en el:

```bash
$ mkdir ~/www/testshopware
$ cd ~/www/testshopware
```

Inicializamos el proyecto:

```bash
$ reward env-init testshopware --environment-type=shopware
```

Creamos el certificado para poder trabajar con HTTPS

```bash
$ reward sign-certificate testshopware.test
```

Cambiamos el directorio raiz a donde se apunta el dominio por “webroot”:

```bash
$ sed -i.old -e 's#^REWARD_WEB_ROOT.*#REWARD_WEB_ROOT=/webroot#' .env
```

Clonamos la plantilla “Production” en el web root donde estará nuestro proyecto:

```bash
$ git clone https://github.com/shopware/production.git ~/www/testshopware/webroot
```

NOTA: El orquestador “_[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_” (y “Warden” también) crean un archivo “.env” en el directorio del proyecto, como [Shopware 6](href="https://www.shopware.com) también requiere de un archivo “.env” con variables de entorno necesitamos que nuestro código este en una sub carpeta del proyecto. En este caso “webroot”

Iniciamos los contenedores del proyecto _[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_:

```bash
$ reward env up
```

Ingresamos al contenedor de la aplicación:

```bash
$ reward shell
```

Cambiamos la versión de <a href="https://getcomposer.org/" target="_blank" rel="noreferrer noopener" aria-label="composer (opens in a new tab)"><em>composer</em></a> a 2:

```bash
$ sudo alternatives --set composer /usr/bin/composer2
```

NOTA: por defecto la versión de _[Reward](href="https://rewardenv.readthedocs.io/en/latest/index.html)_ con la que estoy trabajando instala para el proyecto de tipo “Shopware” composer 1.x pero necesitamos la versión 2.

Instalamos las dependencias con [composer](href="https://getcomposer.org/):

```bash
$ composer install
```

Creamos un archivo “**.env**” con la variable de entorno “**APP_URL**” setada con el dominio local de nuestro proyecto.

```bash
$ echo 'APP_URL="https://testshopware.test"' >> .env
```

Corremos el setup con el parámetro “**&#8211;force**” para, mediante el wizard, setear las otras variables de entorno en el archivo “**.env**”

```bash
$ bin/console system:setup --force
```

Para el &#8220;**host**&#8221; de la base de datos usar “**mysql**” en lugar de “localhost” y “**app**” como **password**.

El ultimo paso es correr el instalador:

```bash
$ bin/console system:install --create-database --basic-setup
```

Si todo salio bien tendremos

Front end: <https://testshopware.test>

Backend : <https://testshopware.test/admin/>

Usuario: admin

Password: shopware

Salimos del contenedor con :

```bash
$ exit
```

Si necesitamos detener el proyecto

```bash
$ reward env stop
```

Y si queremos reiniciarlo

```bash
$ reward env start
```

Para este ejemplo se ha utilizado:

Ubuntu 20.04, Docker 20.10, Reward 0.2.0-beta, Shopware 6.4

Reward: https://rewardenv.readthedocs.io/en/latest/index.html

Shpware 6: https://www.shopware.com