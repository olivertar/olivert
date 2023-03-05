---
title: Shopware 6 – Instalar extensiones / plugins
author: olivertar
type: post
date: 2021-10-01T00:10:02+00:00
url: /shopware-6-instalar-extensiones-plugins/
image: /img/placeholder.jpg
catalog: true
categories:
  - General
  - Shopware
tags:
  - extension
  - extensions
  - plugin
  - shopware
  - shopware 6

---
[Shopware](https://www.shopware.com/en/) 6 tiene diferentes mecanismos para instalar extensiones.

En este post veremos cómo instalar una extensión existente en el [Marketplace de Shopware](https://store.shopware.com/en/)

Vamos a tomar como ejemplo la extensión "Shopware Language Pack" disponible para versiones mayores a 6.3.5.0

https://store.shopware.com/en/swag338126230916f/shopware-language-pack.html

[Shopware](https://www.shopware.com/en/) nos permite instalar de 3 maneras

* Desde el menú "Extensions > Store" del panel de administración de nuestro proyecto

* Descargando el código de la extensión desde el marketplace

* Vía [composer](https://getcomposer.org/)

Voy a mostrar como utilizar [composer](https://getcomposer.org/) que considero es el método profesionalmente más adecuado.

Luego de comprar la extensión vamos a nuestra cuenta en [Shopware](https://www.shopware.com/en/): https://account.shopware.com/

Usamos el menú "Merchant Area > Shops" para seleccionar el &#8220;shop&#8221; para el cual hemos comprado la extensión y luego la extensión.

![screenshot](/wp-content/uploads/2021/10/image-1024x378.png)

![screenshot](/wp-content/uploads/2021/10/image-1-1024x380.png)

![screenshot](/wp-content/uploads/2021/10/image-2-1024x598.png)

Al presionar el botón “_Install via Composer_” se nos abre un popup con un formulario

La primera vez debemos generar el token y guardar la información

![screenshot](/wp-content/uploads/2021/10/image-3.png)

En ese mismo popup se muestra la información necesaria para el setup y la instalación y se explica cómo proceder.

De momento copiamos la información que vamos a necesitar en los próximos pasos.

En la raíz del proyecto donde vamos a instalar la extensión corremos la siguiente linea que agregará el repositorio de <a rel="noreferrer noopener" href="https://www.shopware.com/en/" target="_blank">Shopware</a> a nuestro &#8220;_composer.json_&#8220;

```bash
$ composer config repositories.shopware-packages '{"type": "composer", "url": "https://packages.shopware.com"}'
```

Luego agregamos al &#8220;_auth.json_&#8221; el token que nos permitirá acreditarnos:

```bash
$ composer config bearer.packages.shopware.com "token-obtenidoen-el-paso-anterior"
```

Finalmente con:

```bash
$ composer require store.shopware.com/swaglanguagepack
```

Agregamos la extensión al &#8220;_composer.json_&#8221; y automáticamente se correrá el &#8220;_composer update_&#8221; y todo lo necesario para que la extensión quede descargada y registrada en nuestro proyecto.

Si bien el proceso se puede completar desde el panel de administración pienso que es mejor si nos acostumbramos a hacerlo desde el terminal.

```bash
$ ./bin/console plugin:refresh
```

Nos mostrara la lista de plugins y sus estados

![screenshot](/wp-content/uploads/2021/10/image-4-1024x213.png)


En el caso de nuestro ejemplo necesitamos &#8220;_instalarlo_&#8221; y &#8220;_activarlo_&#8221; para completar la operación. Lo haremos con los siguientes comandos

```bash
$ ./bin/console plugin:install swaglanguagepack 
$ ./bin/console plugin:activate swaglanguagepack
```

Con esto habremos completado la instalación y si vamos al admin de nuestro proyecto podremos ver que la extensión aparece instalada y activada.

![screenshot](/wp-content/uploads/2021/10/image-5-1024x146.png)

Para terminar dejo la lista completa de comandos relacionados con plugins

```
plugin:activate                    Activates given plugins
plugin:create                      Creates a plugin skeleton
plugin:deactivate                  Deactivates given plugins
plugin:install                     Installs given plugins
plugin:list                        Show a list of available plugins.
plugin:refresh                     Refreshes the plugins list in the storage from the file system
plugin:uninstall                   Uninstalls given plugins
plugin:update                      Updates given plugins
plugin:zip-import                  Import plugin zip file.
```
