---
title: Shopware 6 – Configurar el lenguaje de la tienda
author: olivertar
type: post
date: 2021-10-07T21:38:00+00:00
url: /shopware-6-configurar-el-lenguaje-de-la-tienda/
image: /img/placeholder.jpg
categories:
  - General
  - Shopware
tags:
  - extension
  - idioma
  - language
  - shopware
  - shopware 6

---
Muy probablemente los idiomas que vienen instalados por defecto no sean suficientes o los necesarios para nuestro proyecto.

En un [post](https://olivert.com.ar/shopware-6-instalar-extensiones-plugins/) anterior usamos como ejemplo para mostrar como instalar extensiones el &#8220;pack de idiomas&#8221;, partiendo de ese punto vamos a mostrar como, una vez instalada y activada la extensión, configurar [Shopware 6](href="https://www.shopware.com/en/) para utilizar el idioma Español.

El primer paso es configurar cuales idiomas estarán disponibles para los &#8220;canales de venta&#8221; (_Sales Channels_) y en el administrador.

Si vamos a &#8220;_Settings > Extensions_&#8221; y elegimos &#8220;_Language pack_&#8221; encontraremos la lista de idiomas disponibles del pack . 

Acá seleccionamos cuales idiomas queremos en cada canal (_Administration_ y _Sales Channels_)

![screenshot](/wp-content/uploads/2021/10/image-6.png)

Dejamos seleccionado &#8220;Español&#8221; para ambos canales.

Si queremos que &#8220;Español&#8221; sea el idioma por defecto del administrador tenemos que editar el perfil del usuario administrador (&#8220;_Your profile_&#8220;) y cambiar el idioma.

Para cambiar el idioma del front, tenemos que cambiarlo para cada &#8220;Canal de venta&#8221;.

Por defecto el canal de venta web que tiene configurado por defecto [Shopware 6](href="https://www.shopware.com/en/) se llama &#8220;_Storefront_&#8220;. Lo editamos.

![screenshot](/wp-content/uploads/2021/10/image-7.png)

Buscamos en la pestaña &#8220;_General_&#8221; la sección &#8220;_Languages_&#8221; (Idiomas) y agregamos &#8220;Español&#8221; a la lista de idiomas disponibles (removemos otros que no necesitamos) y también seleccionamos del combo &#8220;_Default Language_&#8221; (Idioma por defecto) &#8220;Español&#8221;

![screenshot](/wp-content/uploads/2021/10/image-8.png)

El ultimo cambio es ajustar, en la misma pestaña, la configuración de dominios (_Domain_) editando el registro con el menú de 3 puntos y luego guardando los cambios.

![screenshot](/wp-content/uploads/2021/10/image-9.png)

A partir de ese momento nuestro front-end habrá cambiado de idioma

![screenshot](/wp-content/uploads/2021/10/image-11.png)
