---
title: Vue Storefront + Magento 2 – Instalación de Vue Storefront en local – segunda parte
author: olivertar
type: post
date: 2020-01-21T13:00:06+00:00
url: /vue-storefront-magento-2-instalacion-de-vue-storefront-en-local-segunda-parte/
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
El ultimo paso para completar la instalación de [Vue Sotrefront](https://www.vuestorefront.io) en local es instalar Magento – Vue Storefront y sincronizar los datos.

Clonamos el repositorio

```bash
$ git clone https://github.com/DivanteLtd/mage2vuestorefront.git mage2vs
```

Luego vamos al directorio &#8220;_src_&#8221; y lo instalamos

```bash
$ cd mage2vs/src
$ npm install
```

Este es el punto donde cambiaremos manualmente algunas configuraciones para conectarnos a nuestro backend Magento 2 + Sample data.

Editamos el archivo &#8220;_src/config.js_&#8221; y buscamos la seccion “_magento: {_”

Ahí debemos modificarlo con la información del usuario API de [Magento 2](https://magento.com/) (se explico como crearlo en un articulo anterior)

```
url - http://tu-magento2-url.com/rest/
consumerKey
consumerSecret
accessToken
accessTokenSecret
```

Ahora estamos en condiciones de sincronizar los datos de nuestro backend [Magento 2](https://magento.com/) con la instancia de [Elasticsearch](https://www.elastic.co/) del API de [Vue Sotrefront](https://www.vuestorefront.io)

Para esto, los comandos disponibles que puedes correr en &#8220;_mage2vs/src_&#8221; son:

```bash
$ node cli.js taxrule
$ node cli.js attributes
$ node cli.js categories
$ node cli.js productcategories
$ node cli.js products
```

Creo que se explica por si mismo que hace cada uno, tené en cuenta que cuando sincronices los productos, si estas usando sample data, va a tomar bastante tiempo completar la operación. 

La otra configuración que tenemos que modificar y nos va a permitir sincronizar ordenes, cart, etc (aquella información que no es estática) es la de vue-storefront-api.

Editamos el archivo “_vue-storefront-api/config/local.json_” y también cambiamos los datos del API de [Magento 2](https://magento.com/) en la seccion &#8220;_magento2&#8243;: {_ con los mismos datos que modificamos el archivo anterior.

Si tenemos una instancia corriendo del API de [Vue Sotrefront](https://www.vuestorefront.io), para que los cambios de configuración tengan efecto tenemos que reiniciarla

Primero debes matar los procesos de la instancia y en el directorio del API vue-storefront-api, correr los comandos

Para las versiones de [Vue Sotrefront](https://www.vuestorefront.io) previas a 1.6 era necesario correr el comando o2m para sincronizar las ordenes, esto ya no es necesario si la opción &#8220;useServerQueue&#8221; esta en false (valor por defecto)

```bash
$ docker-compose up -d
$ npm run dev
```

En un próximo articulo veremos algunas consideraciones finales sobre la instalación de [Vue Sotrefront](https://www.vuestorefront.io) en local.

Este es el enlace donde pueden consultar con mas detalle este tema https://docs.vuestorefront.io/guide/installation/magento.html#synchronizing-orders-and-magento-images