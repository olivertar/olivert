---
title: Certificados SSL gratuitos
author: olivertar
type: post
date: 2020-01-20T21:35:35+00:00
url: /certificados-ssl-gratuitos/
image: /img/placeholder.jpg
categories:
  - General
  - Productividad
tags:
  - ssl certificate

---
Hoy día es casi obligatorio que cada sitio web use un certificado SSL para asegurar la comunicación entre el servidor y el cliente.

Como desarrolladores vamos a necesitar que nuestro servidor de staging tenga su propio certificado para poder reproducir las mismas condiciones que tendremos en un eventual servidor de producción.

En otros casos, como cuando trabajamos con PWA o Service Workers el certificado es un requisito para que las cosas funcionen.

Los certificados tienen un costo anual como los tiene un dominio, para evitar este costo en los servidores de desarrollo una buena opción es instalar certificados gratuitos que técnicamente tienen las mismas características y valides que los certificados pagos.

Para el desarrollo de mis proyectos personales tengo contratado un servicio de <a href="https://www.vultr.com" target="_blank" rel="noreferrer noopener" aria-label="Vultr (opens in a new tab)">Vultr</a> ya que necesito pleno acceso al servidor y por sobre todo que sea lo mas barato posible 🙂

En esta instancia tengo deployado un stack LAMP básico al que agregue cosas que fui necesitando (GIT, NODE, etc etc)

El propósito de este articulo es explicar lo simple que es generar e instalar un certificado gratuito en un entorno como este.

Usaremos el servicio de [Let's Encrypt](https://letsencrypt.org/).

Si bien es posible obtener el certificado y luego instalarlo en el servidor, debido al propósito que le vamos a dar al certificado es mas sencillo (exponencialmente mas sencillo!!!) hacerlo con el cliente provisto por <a href="https://letsencrypt.org/" target="_blank" rel="noreferrer noopener" aria-label="Let’s Encrypt (opens in a new tab)">Let’s Encrypt</a>, **_Certbot_**.

Lo primero es agregar el ppa y luego instalar el cliente

```bash
$ add-apt-repository ppa:certbot/certbot
$ apt-get update
$ apt-get install python-certbot-apache
```

 *_Muy probablemente deberían usar sudo si no están trabajando con un usuario Root_

Luego ejecutamos el cliente al que le pasaremos el dominio o subdominio. (reemplazar con el nombre del dominio requerido)

```bash
$ certbot --apache -d subdominio.dominio-ejemplo.com
```

Podemos hacer que el certificado sea valido para distintos subdominios. El ejemplo mas común es que sea valido para el dominio base y el subdominio www

```bash
$ certbot --apache -d dominio-ejemplo.com -d www.domino-ejemplo.com
```

El instalador nos preguntará si queremos que todo el trafico se redirija automáticamente de http a https

```bash
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
 
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
 
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
```

Cuando termine de correr el instalador podes verificar que el certificado fue instalado en la carpeta “_/etc/letsencrypt/live_”

El certificado expira a los 90 días, la ventaja de usar Certbot es que también se ocupa de renovarlo automáticamente.  
Podemos verificar que esto funciona con este comando

```bash
$ certbot renew --dry-run
```

Finalmente podremos verificar la validez y características de nuestro certificado de esta manera:

<https://www.ssllabs.com/ssltest/analyze.html?d=dominio-ejemplo.com&latest>

Yo utilizo Apache como servidor web, pero también es posible instalarlo en NGINX. En ese caso debemos instalar el cliente para NGINX

```bash
$ apt-get install python-certbot-nginx
```

y correrlo con el siguiente comando

```bash
$ certbot --nginx –redirect
```

Ya no tenemos excusa para no usar certificados SSL en nuestros proyectos!!!

  *La aplicación de las instrucciones mostradas en este articulo pueden modificar la configuración del servidor web y eventualmente causar un mal funcionamiento, se recomienda tomar los recaudos necesarios ya que el autor no es responsable por las consecuencias que pueda tener el uso de estos comandos.
