---
title: Shopware 6 – Instalación parte 3 – Plantilla Production
author: olivertar
type: post
date: 2021-05-18T18:07:00+00:00
url: /shopware-6-instalacion-plantilla-production/
image: /img/placeholder.jpg
categories:
  - General
  - Shopware
tags:
  - shopware
  - shopware 6

---
Si necesitamos instalar_[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ para comenzar un proyecto para una tienda de comercio electrónico debemos utilizar la plantilla “**Production**”

Cuando utilizamos esta plantilla necesitamos un entorno que cumpla los requerimientos que tiene _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_. Podemos consultar cuales son en la [documentación](https://developer.shopware.com/docs/guides/installation/overview)

Una opción es configurar el stack local con todos los servicios y características necesarias pero será más simple y provechoso utilizar un ambiente &#8220;dockerizado&#8221;. Por ejemplo [Reward](https://rewardenv.readthedocs.io/en/latest/index.html) o [Warden](https://docs.warden.dev/index.html).

En mi caso estoy utilizando [Reward](https://rewardenv.readthedocs.io/en/latest/index.html). No voy a explicar como instalar el orquestador porque no es el propósito de este post, sea cual sea el entorno &#8220;dockerizado&#8221; que se utilice el punto de partida es el mismo.

Clonar la plantilla “**Production**” en el directorio establecido como **web root**

```bash
$ git clone https://github.com/shopware/production.git ~/www/testshopware/webroot
```

Ingresar al shell del contenedor de la app. En caso de usar [Reward](https://rewardenv.readthedocs.io/en/latest/index.html)

```bash
$ cd ~/www/testshopware/
reward shell
```

Una vez dentro del contenedor usamos [composer](https://getcomposer.org/) para instalar las dependencias

```bash
$ composer install
```

El próximo paso es crear un archivo “**.env**” con la variable de entorno **APP_URL** a la que le asignaremos el dominio local que usaremos para el proyecto. En mi caso &#8220;<https://testshopware.test>&#8220;

```bash
$ echo 'APP_URL="https://testshopware.test"' >> .env
```

Corremos el comando de setup con el parámetro “&#8211;force” que completara la información necesaria en el archivo “**.env**” a través de un wizard 

```bash
$ bin/console system:setup --force
```

Podemos elegir los valores por defecto, en mi caso como utilizo [Reward](https://rewardenv.readthedocs.io/en/latest/index.html) para el **host** de la base de datos en lugar de “**localhost**” debo usar el del entorno, “**mysql**”.

Como **password** para la base de datos usamos “**app**”

Con esto completamos lo necesario en la configuración básica, podemos editar el archivo “**.env**” para hacer algún cambio manual o solo por curiosidad.

El último paso es correr el instalador&nbsp;

```bash
$ bin/console system:install --create-database --basic-setup
```

Cuando termine podemos acceder al frontend en la url configurada como local

<https://testshopware.test>

Y al backend

<https://testshopware.test/admin/>

Usando **admin** y **shopware** como **usuario** y **contraseña**.

Con esto tenemos la plataforma instalada y lista para comenzar a personalizarla para nuestro proyecto específico.