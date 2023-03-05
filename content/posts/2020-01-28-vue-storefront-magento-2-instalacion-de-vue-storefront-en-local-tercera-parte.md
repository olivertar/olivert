---
title: Vue Storefront + Magento 2 – Instalación de Vue Storefront en local – tercera parte
author: olivertar
type: post
date: 2020-01-28T14:23:21+00:00
url: /vue-storefront-magento-2-instalacion-de-vue-storefront-en-local-tercera-parte/
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
Este articulo contiene información complementaria que no fue incluida en artículos anteriores para no distraer la atención del foco principal.

**Detener todas las instancias de [Vue Storefront](https://www.vuestorefront.io)**

En teoría “node killall” debería detenerlas, pero al menos en mi caso eso no sucede.

Si tenes el proceso corriendo en un terminal, &#8220;CTRL + C&#8221; deberia matarlo, pero si esto tampoco resulta la alternativa, siempre hablando de sistemas Linux, es matar los procesos que corren en el puerto 3000 y 8080 (si estos fueron los puertos donde fue lanzado)

Para eso debemos primero determinar el ID de Proceso (PID) y luego matarlo

```
lsof -i:3000
kill -9 PID_3000

lsof -i:8080
kill -9 PID_8080
```

**Como iniciar [Vue Storefront](https://www.vuestorefront.io) en el entorno local.**

Ir al directorio donde esta Vue Storefront API (_vue-storefront-api_) y correr 

```bash
$ docker-compose up -d
$ npm run dev
```

Ir al directorio donde esta [Vue Storefront](https://www.vuestorefront.io) y correr 

```bash
$ npm run dev
```

**[Kibana](https://www.elastic.co/what-is/kibana)**

Cuando instalamos Vue Storefront API, ademas de [Redis](https://redis.io/) y [Elasticsearch](https://www.elastic.co/), [Docker](https://www.docker.com/) levanta una instancia de [Kibana](https://www.elastic.co/what-is/kibana).

Kibana es una herramienta que nos permite ver y explorar los datos indexados en [Elasticsearch](https://www.elastic.co/) lo que puede ser muy útil si nos adentramos en el desarrollo con [Vue Storefront](https://www.vuestorefront.io).

Podemos acceder a [Kibana](https://www.elastic.co/what-is/kibana) en http://localhost:5601/

No es el propósito de este articulo explicar como se utiliza, pero si queremos ver de que se trata necesitamos saber el nombre del index. En nuestro caso es “vue\_storefront\_catalog”

Pienso que en este punto tenemos los conocimientos mínimos para instalar y comenzar a aprender mas en profundidad como usar, modificar e implementar [Vue Storefront](https://www.vuestorefront.io) con [Magento](https://magento.com).

Si continuamos en el viaje de trabajar con Vue Storefront, lo mejor es acostumbrarse a utilizar la documentación que es muy amplia y completa

https://docs.vuestorefront.io
