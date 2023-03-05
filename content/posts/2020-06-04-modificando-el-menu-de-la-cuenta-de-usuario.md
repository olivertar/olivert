---
title: Magento 2 – Modificando el menú de la cuenta de usuario
author: olivertar
type: post
date: 2020-06-04T20:32:03+00:00
url: /modificando-el-menu-de-la-cuenta-de-usuario/
featured_image: /wp-content/uploads/2020/06/customer_account_nav.png
image: /img/placeholder.jpg
categories:
  - Magento
tags:
  - basicos
  - custome account
  - Magento 2
  - menu
  - navigation

---
Es común que a los desarrolladores nos soliciten modificar el menú que se muestra en la sección “My Account”

Lo mas común es remover algún ítem, cambiar el orden o agregar algún ítem nuevo.

Para eliminar o alterar el orden de los ítems lo que tenemos que hacer es agregar información al layout “customer_account.xml” del modulo Magento Customer

La manera de hacerlo es extendiendo el layout “customer_account.xml” en nuestro theme personalizado

Si no existe, tenemos que crear el archivo para el layout en nuestro theme personalizado.

<pre class="wp-block-code"><code>app/design/frontend/Orangecat/custom_theme/Magento_Customer/layout/customer_account.xml</code></pre>

```xml
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>

    </body>
</page>
```

Dentro de las etiquetas “body” es donde colocaremos las “instrucciones” para modificar el menú.

### Eliminar ítems

La manera es usar la propiedad “remove” en el tag “referenceBlock”

En este ejemplo se muestra como eliminar el ítem correspondiente a productos descargables

```xml
<referenceBlock name="customer-account-navigation-downloadable-products-link" remove="true"/>
```

Esta es la lista de “instrucciones” para remover todos los ítems del menú por defecto:

```xml
<!-- Account Dashboard-->
<referenceBlock name="customer-account-navigation-account-link" remove="true"/>

<!-- Account Information-->
<referenceBlock name="customer-account-navigation-account-edit-link" remove="true"/>

<!-- Address Book-->
<referenceBlock name="customer-account-navigation-address-link" remove="true"/>

<!-- My Orders-->
<referenceBlock name="customer-account-navigation-orders-link" remove="true"/>

<!-- My Downloadable Products-->
<referenceBlock name="customer-account-navigation-downloadable-products-link" remove="true"/>

<!-- Newsletter Subscriptions-->
<referenceBlock name="customer-account-navigation-newsletter-subscriptions-link" remove="true"/>

<!-- My Credit Cards-->
<referenceBlock name="customer-account-navigation-my-credit-cards-link" remove="true"/>

<!-- Billing Agreements-->
<referenceBlock name="customer-account-navigation-billing-agreements-link" remove="true"/>

<!-- My Product Reviews-->
<referenceBlock name="customer-account-navigation-product-reviews-link" remove="true"/>

<!-- My Wish List-->
<referenceBlock name="customer-account-navigation-wish-list-link" remove="true"/>
```

### Alterar el orden de los ítems

Para lograrlo necesitamos modificar el argumento “sortOrder” del ítem (o de los ítems si son mas de uno) que queramos cambiar de posición.

```xml
<referenceBlock name="customer-account-navigation-wish-list-link">
    <arguments>
        <argument name="sortOrder" xsi:type="number">20</argument>
    </arguments>
</referenceBlock>
```

### Agregar nuevos ites

Referenciandonos en “customer\_account\_navigation” agregamos nuestro nuevo block. Pienso que el código se explica por si mismo de modo que veamos el siguiente ejemplo.

Si queremos agregar un ítem que permita cerrar la sesión el código seria el siguiente:

```xml
<referenceBlock name="customer\_account\_navigation">  
    <block class="Magento\Customer\Block\Account\SortLinkInterface" name="customer-account-navigation-logout-link">  
        <arguments>
            <argument name="path" xsi:type="string">customer/account/logout</argument>  
            <argument name="label" xsi:type="string">Logout</argument>  
            <argument name="sortOrder" xsi:type="number">1000</argument>  
        </arguments>  
    </block>  
</referenceBlock>
```