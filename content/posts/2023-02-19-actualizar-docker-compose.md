---
title: Actualizar docker-compose
author: olivertar
type: post
date: 2023-02-19T19:21:57+00:00
url: /actualizar-docker-compose/
image: /img/placeholder.jpg
categories:
  - General
  - Productividad

---
Recientemente necesite actualizar la version de [docker-compose](https://docs.docker.com/compose/) para poder actualizar la version de &#8220;[Reward](http://rewardenv.readthedocs.io)&#8220;

Para mi setup Ubuntu 20.04 hice lo siguiente

Verifique la versión con &#8220;docker-compose version&#8221;  
Removí docker-compose probando:

<pre class="wp-block-code"><code class="">$ sudo apt-get remove docker-compose
</code></pre>

Pero como lo habia instalado con CURL tuve que usar:

```bash
$ sudo rm /usr/local/bin/docker-compose
```

Descargue la ultima versión de docker-compose  

https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64

Posicione nuevamente el binario

```bash
$ sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
```

Y le di permisos de ejecución

```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```

Verifique nuevamente la versión de docker-compose

```bash
$ docker-compose version
```
