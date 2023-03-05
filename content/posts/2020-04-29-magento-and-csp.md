---
title: Magento and CSP
author: olivertar
type: post
date: 2020-04-29T13:35:56+00:00
url: /magento-and-csp/
image: /img/placeholder.jpg
categories:
  - Magento
tags:
  - csp
  - Magento 2

---
CSP, &#8220;Content Security Policy&#8221; o &#8220;Politica de seguridad de contenidos&#8221; en español son una serie de reglas que determinan desde que origenes y que tipos de contenidos pueden ser descargados por un cliente web.

Permiten limitar los ataques XSS (cross-site scripting) y la inyeccion de codigo.

En terminos practicos se traducen en cabeceras (headers) que se le envian al browser con cada peticion (request).

Magento 2.3.5 incluye un mecanismo con el cual fácilmente podemos administrar esas reglas.

<https://devdocs.magento.com/guides/v2.3/extension-dev-guide/security/content-security-policies.html>

El modulo involucrado es Magento Csp y la manera simple de implementar o cambiar reglas es mediante un xml, &#8220;csp_whitelist.xml&#8221; que tendremos en el directorio &#8220;etc&#8221; de nuestros modulos.

Hay dos modos &#8220;restrict mode&#8221; y &#8220;report-only&#8221;.  
En el primer caso Magento restringe las peticiones que no cumplen las reglas y en el segundo caso solo emite un reporte que podemos ver en la consola del navegador.  
Este mensaje es muy útil para depuración, sea eliminar la referencia a un script de terceros o permitirlo configurando una regla.

[Report Only] Refused to load the script &#8216;https://www.xxx.com/api.js&#8217; because it violates the following Content Security Policy directive: &#8220;script-src assets.adobedtm.com…&#8221;

Las reglas determinan el tipo de contenido: media-src, style-src, script-src, etc que pueden ser obtenidos desde un dominio (value)

La documentación es completa y la implementación es muy simple.

Ahora solo queda disfrutar de esta nueva característica de Magento 2.