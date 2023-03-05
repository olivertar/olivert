---
title: Como hacer un patch para Magento 2
author: olivertar
type: post
date: 2020-08-10T17:44:09+00:00
url: /como-hacer-un-patch-para-magento-2/
featured_image: /wp-content/uploads/2020/08/patch.jpg
image: /img/placeholder.jpg
categories:
  - Magento
tags:
  - Magento 2

---
Magento 2 tiene métodos que permiten modificar comportamientos determinados por los módulos de core de Magento o módulos de terceros instalados con composer. Plugin, Rewrite, etc

Pero que pasa cuando las modificaciones necesarias son producto de un error (bug) y no de un requerimiento para modificar la plataforma?

En algunos casos es posible mediante los mecanismos de Magento modificar el comportamiento y corregir el bug, pero en otros no y hacer modificaciones al core no es una opción ya que los cambios se perderían (ademas de ser considerado una mala practica) cada vez que corremos composer install.

Desde mi punto de vista personal, cuando se trata de hacer la corrección de un error no corresponde hacer un rewrite o un plugin, lo indicado es hacer un patch ya que se asume que tarde o temprano en versiones posteriores se incluirá una corrección del problema en el core y en caso de actualizar el core de Magento el modulo con la modificación seguirá existiendo en nuestro proyecto aunque no sea necesario.

La finalidad del patch es hacer una corrección física del/los archivo/s que causa/n el error y se aplicará cada vez después de correr composer install.

Hay bugs conocidos para los cuales podemos encontrar patchs en github (normalmente se incorporan al core de Magento cada 3 meses) e instalarlos desde ahí pero en algunos casos necesitamos crear un patch nosotros para un modulo que hemos comprado o un bug de Magento para el cual aun nadie ha creado uno.

Hay varias maneras de generar un patch, yo voy a explicar una que no necesariamente es la mejor o la indicada para todos los casos.

Recientemente tuve un problema con un modulo de MageWorx que me pidieron utilizar para un proyecto y tuve que modificar un archivo que tenia un problema simple.

Primero vamos al directorio donde se encuentra el archivo que queremos patchear y hacemos una copia con otro nombre. Normalmente al nombre del archivo yo agrego la palabra “Fixed”.

```bash
$ cd vendor/mageworx/module-shippingrules/Model/Config/Source/Shipping/
$ cp Methods.php MethodsFixed.php
```

Editamos el archivo “MethodsFixed.php” y hacemos todos las correcciones necesarias para subsanar el problema.

El próximo paso es correr el comando diff para obtener las modificaciones

```bash
$ diff -u Methods.php MethodsFixed.php > mageworx_shippingrules_methods.patch
```

“mageworx\_shippingrules\_methods” es el nombre del archivo con las diferencias y es de libre elección

Los patchs pueden estar en cualquier ruta pero lo habitual es encontrarlos en una carpeta “patches” del directorio raíz del proyecto.

En mi caso uso 2 subcarpetas “core” y “nocore” para diferenciar si los patch corresponden a archivos de Magento o a módulos de terceros.

Esto es útil cuando tenemos que hacer el update del core de Magento 2 y es necesario verificar si los patchs siguen siendo validos.

Entonces, si la carpeta “patches” (y las subcarpetas si lo desean) no existe las creamos y movemos el archivo “.patch”

```bash
$ mv mageworx_shippingrules_methods.patch ~/www/root_project_folder/patches/nocore/
```

Luego editamos el archivo “.patch”

Las 2 primeras lineas mostraran algo similar a:

```
--- Methods.php	2020-08-10 12:06:14.510599032 -0300
+++ MethodsFixed.php	2020-08-10 12:05:17.397000000 -0300
```

cambiar estas lineas por lo siguiente:

```
diff --git a/Model/Config/Source/Shipping/Methods.php b/Model/Config/Source/Shipping/Methods.php
index 3ee2rd..8349152 111644
--- a/Model/Config/Source/Shipping/Methods.php
+++ b/Model/Config/Source/Shipping/Methods.php
```

Notese que agregue antes del nombre del archivo la ruta del archivo dentro del modulo “/Model/Config/Source/Shipping/”

Respecto al hash index, no es algo que sea necesario modificar en principio.

El archivo modificado queda así:

```
diff --git a/Model/Config/Source/Shipping/Methods.php b/Model/Config/Source/Shipping/Methods.php
index 3ee2rd..8349152 111644
--- a/Model/Config/Source/Shipping/Methods.php
+++ b/Model/Config/Source/Shipping/Methods.php
@@ -111,12 +111,10 @@
             );
             $methods&#91;$carrierCode] = &#91;'label' => $carrierTitle, 'value' => &#91;]];
             foreach ($carrierMethods as $methodCode => $methodTitle) {
-                $methods&#91;$carrierCode]&#91;'value']&#91;] = &#91;
-                    'value' => $carrierCode . '_' . $methodCode,
-                    -'label' => '&#91;' . $carrierCode . '] ' . ($methodTitle ? $methodTitle : $methodCode),
-                ];
+                if(is_string($carrierCode) && is_string($methodCode) && is_string($methodTitle)){
+                    $methods&#91;$carrierCode]&#91;'value']&#91;] = &#91;
+                        'value' => $carrierCode . '_' . $methodCode,
+                        -'label' => '&#91;' . $carrierCode . '] ' . ($methodTitle ? $methodTitle : $methodCode),
+                    ];
+                }
             }
         }
```

El patch esta listo, lo próximo es incorporarlo al archivo composer.json que se encuentra en la raíz de nuestro proyecto.

Buscamos la sección de patches y si no existe la creamos.

Para el ejemplo anterior, debería quedar de esta manera:

```
"extra": {
        "magento-force": "override",
        "patches": {
            "mageworx/module-shippingrules": {
                "Correccion del tipo de dato": "patches/nocore/mageworx_shippingrules_methods.patch"
            }
        }
    }
```

En estas lineas se muestra el nombre del modulo, la descripción y el archivo con el patch.

Si no estamos seguros cual es el nombre del modulo que tenemos que usar tenemos que buscarlo en el composer.json del modulo al que pertenece el archivo que queremos patchear.

El ultimo paso es aplicar el patch, lo hacemos con :

```bash
$ composer install
```

Finalmente, si pasado el tiempo instalamos una versión actualizada del modulo (o actualizamos el core de magento) y el patch no es necesario solo tenemos que removerlo del composer.json de nuestro proyecto y eliminar el archivo con el patch que ya no será necesario.