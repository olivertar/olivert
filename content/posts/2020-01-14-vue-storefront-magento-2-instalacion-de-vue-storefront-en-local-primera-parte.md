---
title: Vue Storefront + Magento 2 – Instalación de Vue Storefront en local – primera parte
author: olivertar
type: post
date: 2020-01-14T10:17:44+00:00
url: /vue-storefront-magento-2-instalacion-de-vue-storefront-en-local-primera-parte/
image: /img/placeholder.jpg
categories:
  - Magento
  - PWA
tags:
  - magento
  - Magento 2
  - pwa
  - vue storefront

---
En este articulo comenzaremos a instalar [Vue Storefront](https://www.vuestorefront.io) en el entorno local y conectarlo con una instancia de [Magento 2](https://magento.com/) + sample data mediante el usuario API que configuré en un [articulo anterior](http://olivert.com.ar/vue-storefront-magento-2-configuracion-de-magent).

Los requisitos para la instalación es que tengamos en nuestro entorno:

  * Docker
  * Node JS
  * Yarn
  * Imagemagick

[Vue Storefront](https://www.vuestorefront.io) usa [Redis](https://redis.io/). Por lo tanto si tienen alguna instancia de [Redis](https://redis.io/) corriendo sugiero detenerla para evitar conflictos con los puertos (podríamos modificarlos pero eso agrega complejidad al asunto).

Nuestra instalación consistirá de 3 componentes:

**&#8211; Vue Storefront**

Es la aplicación frontend en si misma hecha con [Vue.js](https://vuejs.org/)

**&#8211; Vue Storefront API**

[Vue Storefront](https://www.vuestorefront.io) no consumirá datos directamente desde nuestra instancia de [Magento 2](https://magento.com/), las categorías, productos, etc son guardados en una instancia de elasticsearch que se levanta (<a href="https://www.docker.com/" target="_blank" rel="noreferrer noopener" aria-label="docker (opens in a new tab)">docker</a> mediante) cuando con este componente. 

**&#8211; Magento – Vue Storefront**

Este componente se encarga de la sincronización de los datos entre [Vue Storefront](https://www.vuestorefront.io) API y nuestra instancia de [Magento 2](https://magento.com/) (backend) y viceversa.

Si bien la instalación se puede hacer manualmente disponemos de un “CLI Installer” (Instalador de linea de comando) que nos simplificará un poco la tarea en lugar de estar modificando manualmente archivos de configuración.

El primer paso es clonar [Vue Storefront](https://www.vuestorefront.io)

```bash
$ git clone https://github.com/DivanteLtd/vue-storefront.git vue-storefront
```

a continuación entramos al directorio y corremos los comandos que descargaran los paquetes necesarios y comenzaran la instalación

```bash
$ cd vue-storefront
$ yarn install
$ npm run installer
```

Lo primero que nos solicitara el instalador es decidir que instancia de [Magento 2](https://magento.com/) usaremos como backend.

Por defecto el instalador usa como backend un demo (http://demo-magento2.vuestorefront.io), si no tenemos una instancia propia de [Magento 2](https://magento.com/) + Sample Data o solo queremos experimentar como se ve [Vue Storefront](https://www.vuestorefront.io) podemos usar esta opción caso contrario debemos usar la URL de nuestra instancia cuando se requiera.

Las otras solicitudes que hace el instalador están relacionadas con los path de tu entorno y la URL desde donde se obtendrán las imágenes. En la mayoría de los casos usar las opciones por defecto pienso que son la mejor opción.

Si todo salio bien deberías ver un mensaje de éxito donde lo importante son las lineas:

```
Storefront: http://localhost:3000
Backend: http://localhost:8080
```

Si pones en un navegador: http://localhost:3000 (o el puerto que indique en el mensaje) deberías poder ver la aplicación y navegar.

Como estamos haciendo una instalación en un entorno de desarrollo por defecto usa el puerto 3000 en lugar del 80 para no tener conflictos con otros servidores web que estén corriendo en el entorno.

Si tienes mas de una instancia de [Vue Storefront](https://www.vuestorefront.io) corriendo veras que el numero de puerto cambia.

Si las instancias adicionales no están en uso o son producto de alguna prueba que hiciste anteriormente pienso que lo mejor es que mates esos procesos para evitarte problemas y solo trabajes con la instancia corriendo en el puerto 3000.

Normalmente deberías poder matar las instancias con:

```bash
$ node killall
```

También notaras que en tu sistema ademas de la carpeta _vue-storefront_ también se ha creado la carpeta _vue-storefront-api._ Es importante que lo sepas porque mas adelante haremos modificaciones de la configuración ahí.

En el próximo articulo veremos como instalar el tercer componente (magento – vue storefront) y la sincronización de información.

También puedes consultar la información de [Vue Storefront](https://www.vuestorefront.io) en ingles para tener mas detalles de todo este procedimiento: https://docs.vuestorefront.io/guide/installation/linux-mac.html