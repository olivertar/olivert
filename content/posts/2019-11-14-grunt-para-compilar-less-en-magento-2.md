---
layout: post
title: Grunt para compilar less en Magento 2
author: olivertar
type: post
date: 2019-11-14T21:20:00+00:00
url: /grunt-para-compilar-less-en-magento-2/
image: /img/placeholder.jpg
categories: [Magento]
tags:
  - Grunt
  - less
  - Magento 2

---
Magento 2, por defecto, utiliza [less](http://lesscss.org/) como lenguaje dinámico de CSS.

Esto significa que debemos compilar el código less para transformarlo en CSS y que sea interpretado por los navegadores.

Magento recomienda el uso de [Grunt](https://gruntjs.com/) para esto. La finalidad de este articulo es mostrar como proceder para instalarlo en un SO Ubuntu y utilizarlo en nuestro proyecto Magento 2.

[Grunt](https://gruntjs.com/) requiere [Node JS](https://nodejs.org/) para funcionar, entonces el primer paso es instalar [Node JS](https://nodejs.org/) si es que no lo tenemos instalado.

```bash
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

El siguiente paso es instalar Grunt en nuestro entorno.

```bash
$sudo npm install -g grunt-cli
```

Si todo salio bien tenemos que configurar Grunt en nuestro proyecto.

Vamos a la carpeta donde esta la raíz de nuestro proyecto:

```bash
$ cd proyecto
```

Ahí, encontraremos 2 archivos que vienen con Magento 2 y deben ser renombrados antes de proceder.

renombrar &#8220;package.json.sample&#8221; como &#8220;package.json&#8221;  
renombrar &#8220;Gruntfile.js.sample&#8221; como &#8220;Gruntfile.js&#8221;

Ahora vamos a actualizar las dependencias de Node JS

```bash
$ sudo npm install grunt --save-dev
$ sudo npm install
$ sudo npm update
$ sudo npm install grunt-contrib-less
```

El siguiente paso es configurar nuestro theme, lo haremos editando el archivo “dev/tools/grunt/configs/themes.js” y en la sección “module.exports = {” agregaremos lo siguiente:

```
customtheme: {
    area: 'frontend',
    name: 'Orangecat/customtheme',
    locale: 'en_US',
    files: &#91;
        'css/styles-m',
        'css/styles-l'
    ],
    dsl: 'less'
},
```

Deberás reemplazar “customtheme” por el nombre del theme que se esta desarrollando.

Antes de activar Grunt recomiendo limpiar los caches y los archivos compilados, yo corro los siguientes comandos para asegurarme que todo quede limpio.

```bash
$ rm -rf pub/static/*
$ rm -rf var/view_preprocessed/*
$ rm -rf var/cache/*
$ rm -rf var/page_cache/*
$ php bin/magento cache:flush
```

El otro punto importante es asegurarse que los directorios “var” y “pub” tienen los permisos adecuados si no los tienen no se podrán compilar los archivos.

Cuando queramos comenzar a trabajar con nuestro theme, debemos correr activar Grunt

```bash
$ grunt exec:customtheme
```

y luego correr el comando

```bash
$ grunt watch
```

que es el que recompilara los archivos necesarios cada vez que se haga una petición al servidor.

Otros comandos utiles son:

```bash
$ grunt clean
$ grunt less
```

El primero vacía los archivos del theme en los directorios “pub” y “var” y el otro compila los archivos del theme.

Para mas detalles pueden consultar la documentacion de Magento 2 en ingles disponible: https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/css-topics/css_debug.html