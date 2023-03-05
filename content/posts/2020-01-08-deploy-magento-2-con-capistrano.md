---
title: Deploy Magento 2 con Capistrano
author: olivertar
type: post
date: 2020-01-08T19:56:49+00:00
url: /deploy-magento-2-con-capistrano/
image: /img/placeholder.jpg
categories:
  - General

---
Para aquellos que no lo sepan, [Capistrano](https://capistranorb.com/) es una herramienta para hacer deploy desarrollada en y para [Ruby](https://www.ruby-lang.org/), no obstante se puede utilizar para hacer deploy de otros idiomas diferentes a [Ruby](https://www.ruby-lang.org/).

En este post vamos a ver como utilizar [Capistrano](https://capistranorb.com/) para hacer un deploy de nuestro proyecto [Magento](https://magento.com/) y correr tareas adicionales post deploy en el servidor de destino.

Una de las caracteristicas de esta herramienta es que genera versiones en el servidor, lo cual permite rapidamente hacer un roll back en caso de que algo no salga bien sin necesidad de hacer cambios en el repositorio.

Los unicos requisitos que debemos cumplir para el servidor remoto son tener GIT instalado y acceso SSH.

Despues del primer deploy encontraremos en el servidor una estructura como esta:

![screenshot](/wp-content/uploads/2019/12/server_tree.png)

En cada deploy Capistrano hace un checkout de la rama del repositorio GIT que hayamos configurado. 

El checkout se hace en la carpeta &#8220;repo&#8221;, copia los archivos a una carpeta nueva dentro del directorio &#8220;releases&#8221;, instala [Magento](https://magento.com/) y realiza todas las operaciones para compilarlo. 

Si todo salio bien, se actualiza un symlink llamado &#8220;current&#8221; para que apunte a la ultima release. El document root del servidor web del host que aloja nuestro proyecto debe apuntar a &#8220;current/pub&#8221; para que todo funcione.

El ultimo punto a tener en cuenta son los archivos que no están incluidos en el repositorio ni son obtenidos con composer durante la instalación. imagenes, sitemaps, env.php etc etc

Para alojar esas carpetas y/o archivos esta la carpeta &#8220;shared&#8221;, lo que hacemos es crear los symlinks necesarios dentro de &#8220;current&#8221;. Podemos crear todos los que necesitemos. Los básicos que requiere [Magento](https://magento.com/) (var/media/env.php) y cualquier otro que por algún motivo requiera nuestro proyecto. Por ejemplo dentro de &#8220;current/pub&#8221;

![screenshot](//wp-content/uploads/2019/12/current_symlink-1024x160.png)

Ahora veamos que necesitamos hacer en nuestra maquina de desarrollo para configurar la herramienta de deploy.

Como pre condición debemos tener instalado [Ruby](https://www.ruby-lang.org/), y 2 gems. 

```bash
$ gem install capistrano
$ gem install capistrano-magento2
```

Al árbol de directorios de nuestro proyecto vamos a agregar la carpeta &#8220;tools/cap&#8221; ahí dentro debemos correr el comando

```bash
$ cap install
```

![screenshot](/wp-content/uploads/2019/12/cap_tree_1-1024x280.png)

Como resultado nuestro árbol quedará como se ve en la imagen

![screenshot](/wp-content/uploads/2019/12/cap_tree_2.png)

Los archivos dentro de &#8220;config/deploy&#8221; corresponde a la configuración especifica para cada entorno donde queramos hacer un deploy. Podemos tener tantos entornos como queramos. 

Dentro de cada uno de estos archivos encontraremos la información de conexión SSH al servidor, el path base donde sera enviado el proyecto y la rama del repositorio que debe usarse para obtener el código entre otras cosas.

Mi archivo &#8220;production.rb&#8221; se ve mas o menos asi:

```
server '45.76.xxx.xxx', user: 'ssh_user', roles: %w{app db web}

set :deploy_to, '/var/www/magentovue/'
set :branch, proc { `git rev-parse --abbrev-ref master`.chomp }
```

El archivo config/deploy.rb tiene la configuracion generica del deploy. Repositorio, parametros para [Magento](https://magento.com/).

Mi archivo &#8220;deploy.rb&#8221; se ve mas o menos asi:

```
set :application, "magentovue"
set :repo_url, "git@bitbucket.org:olivertar/magentovue.git"

set :magento_deploy_languages, ['en_US']
set :magento_deploy_maintenance, "true"
set :magento_deploy_themes, ['Magento/backend', 'Magento/luma']
set :magento_deploy_chmod_d, "0770"
set :magento_deploy_chmod_f, "0660"
set :magento_deploy_composer, "true"
```

El archivo &#8220;Capfile&#8221; es el mas generico y se utiliza para incluir plugins, establecer paths, etc.

Mi archivo Capfile se ve mas o menos asi:

```
# Load DSL and set up stages
require 'capistrano/setup'

# Load Magento deployment tasks
require 'capistrano/magento2/deploy'
require 'capistrano/magento2/pending'

# Load Git plugin
require "capistrano/scm/git"
install_plugin Capistrano::SCM::Git

# Load custom tasks from `lib/capistrano/tasks` if you have any defined
Dir.glob('lib/capistrano/tasks/*.rake').each { |r| import r }
```

En este punto tenemos lista la configuracion que nos permitiria hacer un deploy a los diferentes entornos, pero como queremos tener la opcion de correr tareas post deploy en el servidor vamos a hacer algunos añadidos.

Dentro del directorio &#8220;lib/capistrano/tasks&#8221; agregaremos un archivo &#8220;deploy.rake&#8221;.

Este archivo permite escribir comandos shell que se ejecutaran en el servidor diferenciados por entornos.

Mi archivo deploy.rake se ve mas o menos asi:

```
namespace :deploy do
    task :published do
        on release_roles :all do
		# Staging server
        	if fetch(:stage) == :staging
	            execute "cp /var/www/magentovue/shared/pub/.htaccess /var/www/magentovue/current/pub/.htaccess"
	            execute "cd /var/www/magentovue/ && chown -R magentovue:www-data ./"
	        end

    		# Live server
	        if fetch(:stage) == :production
	            execute "cp /var/www/magentovue/shared/pub/.htaccess /var/www/magentovue/current/pub/.htaccess"
	            execute "cd /var/www/magentovue/ && chown -R magentovue:www-data ./"
	        end
        end
    end
end
```

Notaran que hay comandos especificos para staging y comandos para produccion. En mi caso lo que hago es cambiar el directorio y copiar el archivo .htaccess del directorio compartido al directorio de la version. 

Ademas, hago un cambio de usuario y grupo de archivos para corregir un problema con la instalación especifica de este proyecto de prueba.

De la misma manera podemos crear un archivo sh reuniendo varios comandos y correrlo despues del deploy.

Cuando queramos hacer un deploy debemos ir al directorio tools/caps y correr el comando deploy para un entorno especifico

```bash
$ cd tools/cap
$ cap staging deploy
```

Si necesitamos hacer un roll back el comando será

```bash
$ cap staging deploy:rollback ROLLBACK_RELEASE=201811141333
```

En este post se han explicado las configuraciones básicas, pueden consultar en github por mas opciones y aplicaciones.

<https://github.com/capistrano/capistrano>

<https://github.com/davidalger/capistrano-magento2>