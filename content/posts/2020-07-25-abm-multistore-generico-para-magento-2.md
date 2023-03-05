---
title: ABM multistore genérico para Magento 2
author: olivertar
type: post
date: 2020-07-25T23:02:02+00:00
url: /abm-multistore-generico-para-magento-2/
featured_image: /wp-content/uploads/2020/07/crud1.png
image: /img/placeholder.jpg
categories:
  - Magento
  - Productividad
tags:
  - extension
  - Magento 2
  - module

---
Los últimos meses estuve trabajando en migrar sitios de Magento 1 a Magento 2.

En muchos casos tuve que migrar código que, con mas o menos características, eran simples ABM ([CRUD](https://es.wikipedia.org/wiki/CRUD) en ingles).

Para no estar reinventando la rueda y mejorar la mantenibilidad del código hice un modulo base sobre el que parto cada vez que necesito un ABM.

La premisa fue que sea los lo mas genérico posible y cumpla con los estándares de Magento en cuanto a la implementación de contratos de servicios y demás buenas practicas.

Si bien hay varios ejemplos de CRUD dando vueltas en la red no encontré uno que permitiera almacenar contenido multistore.

Entonces para mi código base utilice una tabla principal donde almaceno contenido y otra tabla donde se guarda la relación contenido-store.

Para los que dan sus primeros pasos con Magento 2 o quieren profundizar un poco mas en como se desarrolla para el backen este es un buen ejemplo ya que involucra muchas de las características mas utilizadas. Setup de esquemas, definición de contratos de servicios, implementación de modelos, generación de grillas y formularios con UI componentes.

Pueden explorar el código fuente en Github y si consideran que seria de utilidad profundizar en la explicación del código los leo en los comentarios.

<https://github.com/olivertar/m2_crud>