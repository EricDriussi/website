---
title: "Cómo no usar Google"
url: ./web-search/
date: 2021-04-25T11:42:43+01:00
draft: true
image: /images/magnify.png
categories:
    - howto
tags:
    - google
    - searx
    - duckduckgo
    - search engine
---

Así que te has dado cuenta de que usar la búsqueda de Google es más o menos como vivir en '_El Mundo Feliz_' de Huxley. Ahora que?

<!--more-->

![coyote](../../../images/coyote.gif)

Sinceramente, no dejé de usar Google por alguna razón profunda o por preocupaciones relacionadas con la privacidad. Simplemente no está a la altura.
Demasiada **propaganda**, demasiados **anuncios** y demasiados resultados **irrelevantes**.

## La vida más allá de Google

Contrario a lo que puedas haber oído, hay un buen par de buscadores disponibles.
Y no se trata de '_memebuscadores_'. Es sólo que no intentan aparentar servir para todo (mientras que solo sirven como **spyware**).
Por ejemplo:

### [Duckduckgo](https://duckduckgo.com)

Probablemente el más popular. Código cerrado pero bastante transparente como compañía. Con sede en los EEUU, sin tracking, sin perfil de usuario, sin tonterías.
Una alternativa general bastante buena, excepto tal vez para búsquedas de imagen.

### [Startpage](https://startpage.com)

Un front-end distinto para Google. La idea es que aún quieres los resultados de Google (por algún motivo) pero preferirías no tener a los de la NSA en casa para cenar.
Éste método [debería](https://github.com/prism-break/prism-break/issu./168) eliminar las chorradas personalizadas de Google.
Tienen su sede en Europa, lo cual tal vez de aporte algo más de confianza.
Aún con eso, al ser de código cerrado tendrás que confiar en ellos y en Google (en menor medida).

### [Swisscows](https://swisscows.com)

Buscador muy 'para todos los públicos'. Filtra todo contenido sexual, violento, pornográfico y demás sin opción a desactivar dicho filtro.
Tiende a usar Bing por detrás pero también hace su propia indexación.
Igual que en el caso anterior, asegura respetar tu privacidad (y desde luego parece ser cierto), pero no siendo de código abierto tendrás que confiar en los Suizos si eso te preocupa.

### [Bing](https://bing.com)

Lo se, confiar en Microsoft en vez de en Google es un poco una tontería. Pero, aunque suene raro, hoy por hoy Microsoft es un jugador 'pequeño' en casi todo salvo sistemas operativos.
Te sigue hasta cuando vas al baño? Sin ninguna duda, pero aún así es preferible que 5 compañías diferentes tengan ciertos datos tuyos a que una sola te conozca mejor de lo que te conoces a ti mismo.
Es útil para tener una perspectiva diferente (puede que te hayas percatado de algunos... Llamémoslos **sesgos**, en los resultados de Google).
Además, es bastante bueno con las imágenes.

### [Yahoo](https://yahoo.com)

El razonamiento anterior aplica aquí más o menos por igual. No deja de ser un poco una porquería de servicio con respecto a la privacidad, pero puede ser útil si entendido correctamente.
Por otra parte, si estás metido en temas de crypto o finanzas en general, tienen un par de herramientas bastante útiles.

### [Yandex](https://yandex.com)

Para aquellos que odian la NSA pero les encantaría conocer al ~~KGB~~ SVR. Con sede en Países Bajos pero Ruso de corazón, parece bastante poco seguro pero puede ser interesante para investigar sobre política. Puedes estar seguro de que no encontrarás mucha **propaganda** Americana ahí.
Es también muy usado en la esfera de influencia Rusa.
No lo usaría sin un proxy o VPN.

## Busca con Searx

'_Y ahora que? Se supone que debo usar seis buscadores en vez de uno?_'

![yesbutno](../../../images/yesbutno.jpg)

Puedes simplemente usar un **meta buscador**, cómo [Searx](https://searx.unixmagick.xyz/)!
Básicamente, lanza consultas a un puñado de buscadores diferentes y te presenta los resultados que obtiene en una única página.

![meta-search](../../../images/meta-search.png)

-   No ofrece resultados personalizados, porque no genera un eprfil sobre ti.
-   No guarda ni comparte lo que buscas.
-   Es [FOSS](https://es.wikipedia.org/wiki/Software_libre_y_de_c%C3%B3digo_abierto) (código [aquí](https://github.com/searx)) por lo que puedes alojar tu propia instancia (como ago yo con el enlace de arriba) o simplemente eligir una en la que confíes de [ésta](https://searx.space/) lista y usarlo.

Si quieres puedes configurar exáctamente cómo y cuales buscadores consulta. Si no, puedes sencillamente usar una instancia así como está.
No funcionará con todos los buscadores disponibles pero probablemente ofrece más de lo que necesitas. Por otra parte es el precio a pagar por la comodidad.
