---
title: Como conectar un modulo de Magento 2 a diferentes instalaciones (y que funcione 100%)
author: olivertar
type: post
date: 2020-07-03T03:58:42+00:00
url: /como-conectar-un-modulo-de-magento-2-a-diferentes-instalaciones/
featured_image: /wp-content/uploads/2020/07/code_slim.jpg
image: /img/placeholder.jpg
categories:
  - Magento
  - Productividad
tags:
  - Magento 2

---
En muchas oportunidades necesitamos probar un modulo de Magento 2 en diferentes versiones o en diferentes proyectos.

Esto es muy común si estamos desarrollando un modulo independiente de algún proyecto especifico o si brindamos soporte para alguna extensión.

En estos casos lo recomendable es que el código del modulo se encuentre fuera de la instalación de Magento y que pueda ser compartido por varias instalaciones mediante enlaces simbólicos (symlinks)

De esta manera, si hacemos alguna modificación al código, esta impactará automáticamente en todos los lugares que estén enlazados.

No es muy complicado de hacer mediante symlinks, Magento tiene una opción de configuración que lo habilita a buscar archivos fuera de la instalación de Magento.

Stores > Configuration > Advanced > Developer > Template Settings > Allow Symlinks 

Esto funciona muy bien excepto en el caso de las plantillas (phtml).

Magento realiza una validación que impide enlaces simbólicos para estos componentes de los módulos. Es claro que no es un error, es una medida de seguridad.

Tratándose de un entorno de desarrollo o en un ambiente de testing, la seguridad no es prioridad y podemos “vulnerar” esas medidas de seguridad para agilizar nuestro trabajo.

Basándome en una vieja versión de “[FishPig](https://github.com/bentideswell/magento2-templatesymlinks)”, desarrollé un modulo muy simple que contempla cambios implementados por Magento en estos años y permite hacer este hack al core de Magento para que las plantillas puedan ser alcanzadas mediante enlaces simbólicos.

https://github.com/olivertar/m2_templatesymlinks