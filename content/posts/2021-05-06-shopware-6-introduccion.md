---
title: Shopware 6 – Introducción
author: olivertar
type: post
date: 2021-05-06T17:55:00+00:00
url: /shopware-6-introduccion/
image: /img/placeholder.jpg
categories:
  - PWA
  - Shopware
tags:
  - api first
  - ecommerce
  - pwa
  - shopware
  - shopware 6

---
[Shopware](href="https://www.shopware.com/en/) es una plataforma de comercio electrónico que está creciendo rápidamente.

Para aquellos que están interesados en conocer o van a trabajar con esta plataforma pienso que es muy valioso conocer el enfoque de desarrollo y como está estructurada la plataforma. Esto no solamente permitirá tener mejor perspectiva al evaluar la plataforma sino que también saber qué es exactamente lo que estamos haciendo cuando lo instalemos.

[Shopware](href="https://www.shopware.com/en/) [on-premise](https://www.ionos.es/digitalguide/servidores/know-how/que-es-on-premises/) es open source y gratuito pero hay versiones pagas que se diferencian por tener características exclusivas basadas en plugins. Esto significa que las versiones Professional y Enterprice comparten el mismo código de la versión gratuita. También existe una versión &#8220;Cloud&#8221; paga.

Tiene un enfoque &#8220;[API First](https://swagger.io/resources/articles/adopting-an-api-first-approach/)&#8221; (traducido al castellano seria &#8220;basado en API&#8221;) 

Open Source, Gratuito y API First son características que busco en un producto porque, desde mi punto de vista, son atributos necesarios para que un producto tenga chances de crecer en el contexto actual.

[Shopware](href="https://www.shopware.com/en/) es una plataforma basada en el framework de [PHP](https://www.php.net/) [Symfony](href="https://symfony.es/).

Recientemente liberaron una nueva versión, [Shopware 6.4](https://www.shopware.com/en/products/shopware-6/), usa <a href="https://symfony.es/" target="_blank" rel="noreferrer noopener" aria-label="Symfony (opens in a new tab)">Symfony</a> 5.2 y el requerimiento mínimo de <a href="https://www.php.net/" target="_blank" rel="noreferrer noopener" aria-label="PHP (opens in a new tab)">PHP</a> es 7.4 aunque ya es compatible con <a href="https://www.php.net/" target="_blank" rel="noreferrer noopener" aria-label="PHP (opens in a new tab)">PHP</a> 8. Que mantengan actualizado los componentes que dan soporte a la plataforma es un buen indicador de la salud del proyecto.

[Shopware](href="https://www.shopware.com/en/) esta estructurado en varios componentes independientes creados con [bundles de Symfony](href="https://www.drauta.com/que-son-los-bundles-de-symfony).

En el centro del sistema se encuentra el componente &#8220;Core&#8221; que es quien provee las APIs además de tener el Framework y la lógica de negocios.

Todos los otros componentes existentes interactúan con el Core a través de sus APIs

**Admin API**, que se utiliza para todas las tareas de administración, es consumida por el &#8220;Administration Component&#8221; (Vue.js SPA).

A través de este API tenemos acceso a todas las entidades de [Shopware 6](https://www.shopware.com/en/products/shopware-6/) y todos los procedimientos ABM (CRUD)

**Store API**, que se utiliza para interactuar con los clientes, es consumida por el &#8220;Storefront component&#8221; y por &#8220;Shopware PWA&#8221;

Otra de las características del componente Core es que tiene un sistema de extensión que nos permite crear plugins, themes y apps.

El código de [Shopware](href="https://www.shopware.com/en/), si bien esta formado por componentes separados, conviven en un mismo [monorepo](href="https://en.wikipedia.org/wiki/Monorepo) en [Github, shopware/platform](href="https://github.com/shopware/platform), donde cada componente tiene su repositorio.

Para su instalación [Shopware](href="https://www.shopware.com/en/) ofrece 2 &#8220;_templates_&#8221; para la instalación, **Production** y **Development**, que elegiremos de acuerdo a lo que necesitemos hacer.  

Tarea &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; shopware/producción&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; shopware/desarrollo

Contribución al core de Shopware&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ❌&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ✅

Desarrollo de extensiones para la tienda&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ✅ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ✅ (preferentemente)

Proyecto personalizado / desarrollo&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ✅ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ❌

Manejo de dependencias / bundles&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ✅&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ❌  


Los &#8220;_templates_&#8221; son repositorios donde están &#8220;_configuradas_&#8221; las dependencias y herramientas necesarias para cada tipo de propósito de instalación.

Development: https://github.com/shopware/development

Production: https://github.com/shopware/production

Otra de las funciones que cumplen los &#8220;_templates_&#8221; es brindar todo lo necesario para las diferentes propuestas de entorno de instalación que nos da <a href="https://www.shopware.com/en/" target="_blank" rel="noreferrer noopener" aria-label="Shopware (opens in a new tab)">Shopware</a>: Docker, Dockware, Valet+, MAMP y from-scratch

Es en este punto que se recomienda mirar un poco algunos archivos de los &#8220;_templates_&#8220;: composer.json y docker-compose.yml&nbsp;

Pienso que esta es la información mínima necesaria para encarar el próximo paso que es la instalación de <a rel="noreferrer noopener" aria-label="Shopware 6 (opens in a new tab)" href="https://www.shopware.com/en/products/shopware-6/" target="_blank">Shopware 6</a> en nuestro entorno local.