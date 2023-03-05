---
title: Vue Storefront + Magento 2 – Configuracion de Magento
author: olivertar
type: post
date: 2020-01-08T19:57:13+00:00
url: /vue-storefront-magento-2-configuracion-de-magento/
image: /img/placeholder.jpg
categories:
  - General

---
Para que nuestra instalación de [Magento](https://magento.com/) este en condiciones de suministrar datos a <a href="https://www.vuestorefront.io/" target="_blank" rel="noreferrer noopener" aria-label="Vue Storefront (opens in a new tab)">Vue Storefront</a> tenemos que hacer 2 cosas:

### Instalar modulo vsbridge indexer

Usamos composer y después la secuencia de comandos habitual para actualizar [Magento](https://magento.com/)

```bash
$ composer config repositories.divante vcs https://github.com/DivanteLtd/magento2-vsbridge-indexer
$ composer require divante/magento2-vsbridge-indexer
$ bin/magento setup:upgrade
```

### Crear Usuario API Rest

En el backend de la instalación de [Magento](https://magento.com/) que suministrará los datos vamos a System > Integrations y usamos el botón &#8220;Add New Integration&#8221; para crear una nueva integración

![screenshot](/wp-content/uploads/2019/12/magento_integration_menu.png)

Tenes que completar la siguiente información

  * Name (Nombre para la nueva integracion)
  * Password (Tu password de usuario admin)

![screenshot](/wp-content/uploads/2019/12/magento_integration.png)

  * En el tab API marcar estos recursos: 
      * Catalog
      * Sales
      * My Account
      * Carts
      * Stores > Settings > Configuration > Inventory Section
      * Stores > Taxes
      * Stores > Attributes > Product
  * Save

![screenshot](/wp-content/uploads/2019/12/magento_integration_api.png)

El sistema te mostrará 4 tokens, tenes que guardarlos porque serán necesarios mas adelante.

![screenshot](/wp-content/uploads/2019/12/magento_tokens-1024x282.png)

Si todo funcionó bien, estamos en condiciones de seguir adelante instalando las otras partes del sistema.