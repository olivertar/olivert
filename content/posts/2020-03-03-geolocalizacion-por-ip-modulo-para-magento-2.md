---
title: Geolocalización por IP – Modulo para Magento 2
author: olivertar
type: post
date: 2020-03-03T21:48:22+00:00
url: /geolocalizacion-por-ip-modulo-para-magento-2/
featured_image: /wp-content/uploads/2020/03/geolocalizacion.jpg
image: /img/placeholder.jpg
categories:
  - Magento
tags:
  - extension
  - magento
  - Magento 2

---
Recientemente hice una prueba técnica para aplicar a un empleo, en uno de los requerimientos se solicitaba geolocalizar al usuario partiendo de su IP.

Esto me dio pie para desarrollar un [modulo](https://github.com/olivertar/m2_geoip) para [Magento 2](href="https://magento.com/) que hace tiempo tenia ganas de hacer. (https://github.com/olivertar/m2_geoip)

**Un poco de historia**

En las épocas doradas de [Magento](https://magento.com/) 1 este tipo de módulos florecían como hongos, la gran mayoría utilizaban un servicio de terceros que permitía conocer la procedencia de una IP. El mas conocido es <a href="https://dev.maxmind.com/geoip/geoip2/geolite2/" target="_blank" rel="noreferrer noopener" aria-label="MaxMind (opens in a new tab)">MaxMind</a>.

Originalmente estos servicios brindaban un archivo en formato CSV y mas cerca en el tiempo comenzaron a usar formato MMDB con la información de las IP de todo el mundo.

Los módulos consistían en un sistema que obtenía periódicamente el archivo desde [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) y luego mediante una librería extraer la información de la IP sea del CSV o del MMBD.

En general las variantes en los módulos consistían en que algunos, después de obtener los archivos, metían la información en tablas de la base de datos de [Magento](https://magento.com/) para mejorar la performance evitando abrir un archivo físico o evitar usar/instalar alguna librería especial de PHP en el caso de los archivos MMDB.

Con la aparición de [Magento 2](https://magento.com/) cambio la codificación pero la arquitectura permaneció mas o menos igual. Quizá la mayor diferencia fue que los módulos de este tipo mayoritariamente dejaron de ser gratuitos.

Durante el año 2019, [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) cambio su política, si bien continua siendo un servicio gratuito en su versión lite, para suministrar la database de IP’s y requiere registrarse y obtener una key para poder hacer la descarga del archivo.

**Opciones gratuitas en este momento**

Al momento de escribir este articulo hay al menos 2 módulos gratuitos de empresas reconocidas que permiten geolocalizar una IP. Ambos utilizan el servicio de [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/).

Revisando el código de 2 módulos encontré que el archivo es descargado desde una url del servidor del desarrollador y no directamente desde el servidor de [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) que requiere la licencia.

Los motivos y la valoración de la razón por la cual lo han hecho de esa manera no es algo que me corresponda criticar, tampoco lo es la manera en que están codificados, pero no me parece apropiado instalar en el sistema de un cliente un modulo que obtenga información desde un servicio que no es el original. De echo lo que se obtiene desde estos servidores “alternativos” es el archivo MMBD en lugar del tar.gz que suministra [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/).

Es por todo esto que me pareció una buena oportunidad para hacer un desarrollo que permita obtener el archivo directamente desde [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) para geolocalizar IP’s.

**Mi versión de modulo Geo IP**

La principal característica de <a href="https://github.com/olivertar/m2_geoip" target="_blank" rel="noreferrer noopener" aria-label="mi modulo Geo IP (opens in a new tab)">mi modulo Geo IP</a>, como se imaginaran, es provee los medios en el backend para guardar la key suministrada por [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) en el registro y poder descargar la información.

El modulo descomprime el archivo tar.gz descargado, guarda solo el archivo mmbd que se encuentra en su interior y descarta el resto.

Las consultas para geolocalizar la IP se hacen directamente sobre el archivo mmdb por lo cual se requiere tener instalada la librería <a href="https://github.com/maxmind/GeoIP2-php" target="_blank" rel="noreferrer noopener" aria-label="GeoIP2 PHP API (opens in a new tab)">GeoIP2 PHP API</a>.

Adicionalmente el archivo con la database de IP’s puede ser actualizado manualmente desde el backend o rutinariamente con el cron. [MaxMind](href="https://dev.maxmind.com/geoip/geoip2/geolite2/) actualiza el archivo una vez por semana.

Para poder ser utilizado por otros módulos de aplicación concreta (switch de moneda, idioma, etc), el modulo suministra un helper que tiene métodos para identificar la IP del usuario y otros para devolver la geolocalizacion a partir de una IP que se pasa como parámetro.

Finalmente, para poder hacer verificaciones el modulo suministra la opción de ingresar una IP de test en el backend que sobreescribirá la IP real del usuario y también brinda la opción de pasar la IP como parámetro en la URL.

**Posibles mejoras**

La mejora que tal vez sea mas importante en sistemas que hagan uso intensivo del servicio sea mover la información a la database de [Magento](https://magento.com/) para mejorar la velocidad de las consultas y sacar ventajas de los caches de la database.

Si alguien tiene alguna sugerencia pueden enviármela para que la evalúe.

**Cuidado!**

Si tu sistema esta instalado detrás de un Firewall, use el servicio de [Cloudflare](https://www.cloudflare.com) o [NGINX](https://www.nginx.com/) es muy posible que el sistema detecte una IP que no sea la IP publica de la visita.

Esto no es un problema causado o que puede ser resuelto por el código del modulo, lo advierto porque es bastante común que nuestro sistema funcione a la perfección en nuestro servidor de staging y sea un desastre cuando lo enviamos a producción.

**Instalación**

Si bien se puede hacer la instalación manualmente (https://github.com/olivertar/m2_geoip), yo recomiendo usar composer ya que se encarga de instalar las dependencias automáticamente (<a rel="noreferrer noopener" aria-label="GeoIP2 PHP API (opens in a new tab)" href="https://github.com/maxmind/GeoIP2-php" target="_blank">GeoIP2 PHP API</a>).

```bash
$ composer require orangecat/geoip
```

El módulo fue desarrollado en [Magento](https://magento.com/) versión 2.3.4 y probablemente no sea compatible con versiones anteriores.

Espero que el módulo pueda ser útil para algún desarrollador y/o sirva de inspiración para algún otro desarrollo.


**Referencias**

[GeoLite2 Free Downloadable Databases](https://dev.maxmind.com/geoip/geoip2/geolite2/)

<https://github.com/olivertar/m2_geoip>

<https://github.com/maxmind/GeoIP2-php>