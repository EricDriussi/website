---
title: "Bitácora #07 - DTOs secos"
url: ./logbook07/
date: 2021-05-27T20:03:23+01:00
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
---

Sobre cuándo repetir y por qué no hacerlo.

<!--more-->

Si se supone que tenemos que reducir el código duplicado, ¿qué carajo hacemos con los DTO?

![logbook](../../../images/ship.gif)

## D.R.Y.

Del Inglés "_dont' repeat yourself_", es un principio según el cual no debemos repetir lógica o procesos, o intentar evitarlo en la medida de lo posible.
No viene siendo muy diferente a lo que conseguimos al encerrar cierta lógica en una función para luego llamarla en varios puntos del código, en vez de repetir la lógica constantemente.

Se atribuye a menudo a Andrew Hunt y David Thomas en su libro "_The Pragmatic Programmer_".

### Redundancia en procesos

Para asegurar que el número de acciones necesarias para conseguir un resultado sean mínimas, **DRY** nos dice que debe haber una sola manera de realizar una determinada tarea.

### Redundancia en lógica

Para asegurar que no haya código repetido (o al menos minimizarlo), tiraremos de abstracción delegando a cada entidad **únicamente** la lógica que le corresponda.

## D.T.O.

Del Inglés "_data transfer object_", es un tipo de objeto usado para encapsular (y transferir, serializar y mutar) información.

La idea básica es que, en un sistema tipo cliente-servidor, las llamadas de uno a otro son 'caras'.
Usando un modelo estándar para enviar toda la información necesaria, con la lógica básica para interpretarla y **nada más**, maximizamos la eficiencia del proceso.

Cabe mencionar sin embargo que, la conversión entre objetos de dominio y DTO (y de vuelta) puede ser un proceso costoso, engorroso y complicado.
En realidad lo que estamos haciendo al emplear éste patrón es desplazar la complejidad donde se entiende más conveniente.

Por eso mismo deberemos asegurarnos de estar haciendo eso y no justamente **lo contrario**.
Aquí hay un '_rabbit hole_' bastante interesante. Empezar por [aquí](https://martinfowler.com/bliki/LocalDTO.html).

## D.R.Y.D.T.O.

![ymca](../../../images/ymca.gif)

¿Que ocurre entonces cuándo nos vemos en el muy común caso de tener varios DTOs con mucho código en común?

Bueno, siguiendo el principio DRY tiramos de abstracción y san se acabó!
Salvo que acabamos de meter la pata.

A menos que las entidades correspondientes a los DTO en cuestión respondan a normas de abstracción muy concretas y estrictas, los DTO no deberán hacerlo.
Al fin y al cabo existen para transferir datos, no para inventarse relaciones entre ellos.

Aún peor, en caso de añadir, eliminar o modificar campos o entidades en el futuro, lo último que queremos es tener que desenredar un nudo de DTOs para luego volverlo a montar.

Ahí está justamente la clave: ese código puede estar repetido **ahora**, pero nada indica que vaya a permanecer así en el **futuro**.

Si realmente puede reducirse el código repetido en los DTOs, lo más probable es que el problema venga de más abajo.
Por otra parte, puede que la coincidencia de código sea meramente **circunstancial**, y que en una semana deje de existir.
La idea de DRY es evitar la duplicación de **lógica** y estructuras, no **necesariamente** de código.

### Conclusión

DRY si, pero hay que saber entenderlo.
DTOs si, pero hay que saber hacerlos.

![typing](../../../images/typing.gif)
