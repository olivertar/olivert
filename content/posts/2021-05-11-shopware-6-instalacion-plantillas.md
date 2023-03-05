---
title: Shopware 6 – Instalación parte 1 – Plantillas
author: olivertar
type: post
date: 2021-05-11T18:00:00+00:00
url: /shopware-6-instalacion-plantillas/
image: /img/placeholder.jpg
categories:
  - General
  - Shopware
tags:
  - shopware
  - shopware 6

---
Si queremos instalar _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ y recurrimos a la <a rel="noreferrer noopener" aria-label="documentación (opens in a new tab)" href="https://developer.shopware.com/docs/guides/installation/overview" target="_blank">documentación</a> veremos que mencionan la configuración de plantillas.

Tienen dos &#8220;**templates**&#8221; diferentes: &#8220;**Production**&#8221; y &#8220;**Development**&#8220;

Pero qué son y para qué se usan?

Lo que llaman &#8220;**templates**&#8221; son en realidad repositorios y el propósito de cada una es brindar facilidades al desarrollador según el propósito para el que se instale _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_.

El “**template**” &#8220;**Development**&#8221; es indicado para desarrolladores que quieren contribuir con código a _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ (contribuciones al core, extensiones genéricas) o por cualquiera que necesite correr _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ con datos de prueba sea por curiosidad o capacitación.

Las características principales del “template” Development son:

  * Levantar un ambiente docker con todo lo necesario para correr Shopware 6. Lo cual nos evita configurar nuestro stack local con todo lo necesario.
  * Tiene un ejecutable (.phar) que nos permite en forma sencilla y rápida (sin utilizar ninguna interfaz web) instalar una versión de Shopware 6 que incluye datos de prueba.

El “**template**” &#8220;**Production**&#8221; es el indicado para el desarrollo de proyectos completos como ser la tienda de una empresa mediante la personalización de los componentes de _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_.

La otra utilidad es crear nuevos componentes para _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_.

En este caso no se incluye la opción para el ambiente dockerizado ni un instalador de un solo click con datos de prueba.

No obstante la instalación se puede realizar mediante comandos de consola sin recurrir a la interfaz web.

NOTA: _[Shopware 6](https://www.shopware.com/en/products/shopware-6/)_ también nos brinda la posibilidad de obtener desde la pagina de descargas (<https://www.shopware.com/en/download/#shopware-6>) un archivo comprimido con todas las dependencias necesarias para instalarlo mediante la interfaz web.

Esta modalidad es desaconsejada para tareas de desarrollo de proyectos reales y queda reservada casi exclusivamente para aquellos que no tienen un mínimo manejo de docker o el uso del terminal. La contrapartida de esta “facilidad” es que es necesario instalarlo en un ambiente que cumpla todos los requisitos.