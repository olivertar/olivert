---
layout: post
title: Agregar paquetes adicionales a Reward
author: olivertar
type: post
date: 2023-02-19T21:22:49+00:00
published: true
url: /agregar-paquetes-adicionales-a-reward/
image: /img/placeholder.jpg
categories: [ General ]
---
En varias ocasiones encontré que para algún proyecto en el que usaba &#8220;[Reward](rewardenv.readthedocs.io)&#8221; como entorno de desarrollo local era necesario agregar paquetes.

Por ejemplo para poder correr [magepack](https://github.com/magesuite/magepack) es necesario tener todas las librerías de Chromium que no están en la imagen por defecto.

Hasta antes de la versión 0.4.0 de &#8220;[Reward](rewardenv.readthedocs.io)&#8221; era necesario agregarlas ejecutando los comandos en cada ocasión en que lanzábamos el entorno del proyecto.

Para usar [magepack](https://github.com/magesuite/magepack) (pero también para [critical CSS](https://developer.adobe.com/commerce/frontend-core/guide/css/critical-path/)) tenia que ejecutar dentro del contenedor cada vez:

```bash
$ sudo apt-get install -y \
    libx11-xcb1 \
    libxcomposite-dev \
    libxcursor1 \
    libxdamage1 \
    libxi6 \
    libxtst-dev \
    libnss3 \
    libcups2 \
    libxss1 \
    libxrandr2 \
    libasound2 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libpangocairo-1.0-0 \
    libgtk-3-0
```

Bien, no parece ser nada muy complicado, pero no es lo ideal y es poco deseable si lo que necesitamos es darle a cada desarrollador la posibilidad de montar el entorno local con la menor cantidad de operaciones y conocimientos posible.

A partir de de la versión 0.4.0 (0.4.1 es la versión actual en el momento de escribir este post) hay un mecanismo que nos permite que cada proyecto tenga su propio [Dockerfile](https://docs.docker.com/engine/reference/builder/).

La realidad es que mi escenario es bastante simple pero el potencial de esta nueva funcionalidad permite una flexibilidad muy grande y que solucionaría escenarios mucho mas complejos sin necesidad de crear nuevas imágenes.

No solo el conocimiento y tiempo para crear un [Dockerfile](https://docs.docker.com/engine/reference/builder/) es mucho menor que el necesario para crear una imagen sino que la facilidad del mantenimiento y modificación es mucho mas fácil en el caso del [Dockerfile](https://docs.docker.com/engine/reference/builder/). 

Bien retomando el ejemplo veamos como se implementa.

Primero creamos un archivo &#8220;reward-env.yml&#8221; dentro de la carpeta &#8220;.reward&#8221;

```bash
$ nano .reward/reward-env.yml
```

Cuyo contenido será:

```
services:
  php-fpm:
    build:
      context: .
      dockerfile: .reward/Dockerfile
```

Para el próximo paso necesitamos averiguar, si no lo conocemos, cual imagen utiliza nuestro proyecto, lo haremos con el siguiente comando:

```bash
$ reward env config
```

Buscamos en la respuesta la linea que corresponde a &#8220;php-fpm > image&#8221;

![screenshot](/wp-content/uploads/2023/02/reward_image_name.png)

Luego creamos, también dentro de &#8220;.reward&#8221;, el archivo &#8220;[Dockerfile](https://docs.docker.com/engine/reference/builder/)&#8220;

```bash
$ nano .reward/Dockerfile
```

Cuyo contenido será (_para el ejemplo propuesto de_ [magepack](https://github.com/magesuite/magepack)):

Notese que en &#8220;FROM&#8221; use el valor obtenido de la &#8220;image&#8221;

```
FROM docker.io/rewardenv/php-fpm:8.1-magento2

USER root

RUN apt-get update && apt-get install -y \
    libx11-xcb1 \
    libxcomposite-dev \
    libxcursor1 \
    libxdamage1 \
    libxi6 \
    libxtst-dev \
    libnss3 \
    libcups2 \
    libxss1 \
    libxrandr2 \
    libasound2 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libpangocairo-1.0-0 \
    libgtk-3-0

USER www-data
```

Cuando tenemos todo listo lanzamos el entorno con el parámetro &#8220;&#8211;buil&#8221;

```bash
$ reward env up --build
```

Veremos como al levantar el entorno se ejecutan los comandos contenidos en el [Dockerfile](https://docs.docker.com/engine/reference/builder/) y nuestro entorno queda preparado para usarlos desde el primer momento.

Enlace a la documentación de Reward: https://rewardenv.readthedocs.io/en/latest/customization/additional-packages.html