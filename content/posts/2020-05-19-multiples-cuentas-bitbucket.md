---
title: Múltiples cuentas Bitbucket
author: olivertar
type: post
date: 2020-05-19T18:52:55+00:00
url: /multiples-cuentas-bitbucket/
image: /img/placeholder.jpg
categories:
  - General
  - Productividad

---
Si bien el artículo se refiere a [Bitbucket](https://bitbucket.org/), es valido para cualquier sistema de repositorios GIT que requieran autenticación SSH.

Si bien es posible tener un único usuario en [Bitbucket](https://bitbucket.org/) y darle acceso a diferentes workspaces, es habitual necesitar una cuenta personal de Bitbucket para nuestros proyectos y otra (u otras) cuentas de la empresa donde trabajamos o repositorios de clientes.

El problema es que una misma clave SSH no puede estar asociada a mas de un usuario.

La solucion es bastante simple, consiste en tener 2 claves SSH en nuestra computadora y asignarlas cada una a un usuario en [Bitbucket](https://bitbucket.org/)

La manera de generar una clave en Linux es con el comando "[ssh-keygen](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html)" dentro del directorio oculto "~/.ssh"

Si ya tienen una clave actualmente, se debe crear una nueva con un nombre diferente

```bash
$ ssh-keygen -f ~/.ssh/second_id_rsa -C "secondsshkey"
```

El siguiente paso es crear un archivo "config" dentro del mismo directorio ("~/.ssh")

En este archivo estableceremos los "alias" para saber en que caso usar una u otra clave

```
Host bitbucket.org
  Hostname bitbucket.org
  IdentityFile ~/.ssh/id_rsa

Host bitbucket.olivertar
  Hostname bitbucket.org
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/second_id_rsa
```

El primer HOST se llama como el host por defecto de de <a rel="noreferrer noopener" href="https://bitbucket.org/" target="_blank">Bitbucket</a> (bitbucket.org) y en caso de invocarse usara la clave SSH por defecto (id_rsa)

Pero si invocamos el host usando el segundo alias (bitbucket-2) se utilizara la otra clave SSH (second\_id\_rsa)

Estos son ejemplos de como usar los alias en el comando "clone"

```bash
$ git clone git@bitbucket.org:usuario-de-mi-trabajo/repositorio-proyecto-1.git
$ git clone git@bitbucket.olivertar:olivertar/repositorio-proyecto-2.git
```

En caso que ya tengan repositorios clonados y necesiten modificar la configuracion para implementar el nuevo usuario hay que modificar la URL del “origin” en el archivo de configuracion del repositorio (“.git/config”))