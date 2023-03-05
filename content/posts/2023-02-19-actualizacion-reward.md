---
title: Actualización Reward a 0.4.1
author: olivertar
type: post
date: 2023-02-19T19:24:34+00:00
url: /actualizacion-reward/
image: /img/placeholder.jpg
categories:
  - General
  - Productividad

---
Desde hace un buen tiempo que estoy utilizando para los desarrollos de aplicaciones PHP &#8220;[Reward](http://rewardenv.readthedocs.io)&#8220;, no me siento calificado para decir si es el mejor, el 2 mejor o que tan bueno es pero desde que lo estoy usando no he tenido ningún problema, me permite utilizar simultáneamente diferentes proyectos, correr diferentes aplicaciones (no solo Magento que es lo que habitualmente desarrollo), cubrió sobradamente todas mis necesidades de configuración de diferentes proyectos y por sobretodo y lo más importante para mi es que cada vez que recurrí a la comunidad por ayuda en Slack siempre recibí respuesta rápida y precisa.

Mi reconocimiento para [János Mikó](https://hu.linkedin.com/in/janosmiko), porque para hacer un buen software seguro hay que ser talentoso pero para mantener una comunidad se necesita más que talento.

En este caso me toco actualizar la versión, en enero de 2023 salió una nueva versión la 0.4.0 que es una re escritura completa de la aplicación.

Lo que me decidió a probarlo, además de ayudar a la comunidad a depurar eventuales bugs, es que se incorporaba una funcionalidad que estaba esperando desde hace tiempo.

Si ya tienen &#8220;[Reward](http://rewardenv.readthedocs.io)&#8221; instalado, la actualización es muy simple.

Según como haya sido instalado &#8220;[Reward](http://rewardenv.readthedocs.io)&#8220;:

```bash
$ reward self-update
```

o

```bash
$ sudo reward self-update
```

Luego

```bash
$ reward install
```

Si todo salio bien tendremos un mensaje como este:

![screenshot](/wp-content/uploads/2023/02/upgrade_reward_ok.png)

En mi caso no salio al primer intento y me mostró este error:

![screenshot](/wp-content/uploads/2023/02/upgrade_reward_error.png)

El problema fue la versión de [docker-compose](https://docs.docker.com/compose/) que era algo antigua (se requiere versión >= 1.26), la actualice y el instalador corrió perfectamente.

Luego solo quedó verificar que los proyectos que ya tenía creados siguieran funcionando y así fue.

Por si a alguien lo necesita [aquí](https://olivert.com.ar/actualizar-docker-compose/) dejo como hice para actualizar &#8220;docker-compose&#8221; en mi caso particular.
