---
title: "Bases del Código Limpio"
url: ./cleancodebasics/
date: 2021-03-24T14:52:21Z
draft: true
image: /images/uncleBob.jpg
categories:
    - tldr
tags:
    - codigo limpio
    - tio bob
    - Robert C. Martin
---

<!--more-->

<p align="center">
	Esto no es el libro más grandioso del mundo, no.
	<br>
	Esto es solo un tributo.
	<br>
	<br>
    <sub><sub>Tiene más gracia en inglés</sub></sub>

</p>

![tenaciousd](../../../images/tenacious.gif)

Saqué la idea de [aquí](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29), aunque los conceptos de abajo surgen fundamentalmente de lectura horizontal y de las charlas del Tío Bob.

---

Dejemos una cosa clara: soy un lector **horrible**.

Puedo pasarme el día escuchando podcasts o siguiendo lecciones y charlas pero es muy raro que encuentre el tiempo (o la atención) para sentarme a leer un libro.
Como mucho me encontrarás leyendo cuando investigo un tema y aún ahí será fundamentalmente lectura horizontal.

Así que ten eso en mente antes de fiarte de mi palabra acerca de la piedra angular que es éste libro.

---

## Introducción

Antes de adentrarnos demasiado en la madriguera del conejo, voy a darte una base intuitiva acerca de lo que va todo este tema.

### Novatos

Simplemente intenta que tu código sea '_bonito_'.

Mira las sangrías, ¿Tienen sentido? ¿Hay como 5 indentadas una dentro de la otra? ¿Usas nombres descriptivos? ¿Hay alguna manera más eficiente de lograr lo mismo? ¿Es elegante tu código?
¿Te sientes orgulloso de él y tienes ganas de enseñárselo a tus compañeros?

### Intermedio

¿Conseguiste que tu código funcionara? Chachi! Eso lo hace comprensible para la máquina. Ahora tienes que volver a andar el camino y hacerlo comprensible para humanos.

Ésta idea de que deberíamos escribir código funcional y limpio del tirón es un sinsentido. Los humanos no realizamos procesos mentales analíticos, nuestro cerebro es un desastre cuando intentamos pensar en nuevas ideas (esa es justamente la gracia). Primero haz que funciona, luego límpialo.

> No has terminado cuando funciona, has terminado cuando es correcto.
> — <cite>Robert C. Martin<cite>

El código es limpio si puede ser comprendido fácilmente por un lector competente. Otro desarrollador debería poder mejorarlo (o arreglarlo de echo).

![wtfdoor](../../../images/wtfdoor.png)

Las sorpresas pueden molar en la vida real pero no me apetece soltar un **"¿Que coño?"** al leer tu código. A más predecible mejor.

> Un código limpio hace una cosa y la hace bien.
> — <cite>Bjarne Stroustrup <cite>

Igual que con la filosofía Unix, el buen software debería hacer **una cosa** y hacerla bien.
En éste contexto esa _cosa_ es similar al concepto de **Mónada** de G. Leibniz.
Hablando en plata: si no puedes refactorizar tu código coherentemente en trozos más pequeños, esa es la 'cosa'.

## Análisis

Vamos a intentar desgranarlo al máximo.

![thinking](../../../images/thinking.gif)

### General

1. Las convenciones están ahí por un motivo. Me es más fácil entenderte si ambos seguimos una serie de normas comunes.
2. KISS (Keep it simple stupid). Reduce la complejidad tanto como puedas, mantén la palabra _minimalismo_ en mente.
3. Encuentra la causa originaria. No te limites a solucionar el problema, resuelve del todo la situación.

### Comprensibilidad

1. Se consistente. Si la clase A tiene un `var name` y un `var surname`, la clase B no debería tener un `var firsName` y un `var lastName` en su lugar.
2. Usa variables explicatorias y no te preocupes por la longitud de los nombres.
3. Prioriza objetos frente a tipos primitivos. Habrá menos partes móviles.
4. Evita condicionales negativos y en general ten cuidado con los condicionales, se van de las manos rápido.

### Nombres

1. Deberían ser descriptivos, precisos, significativos, pronunciables y '_buscables_'.
2. No uses `data` o `object` en el nombre. Ya lo sabemos.

### Funciones

1. Pequeñas.
2. Hacen una cosa.
3. Usa nombres descriptivos, posiblemente verbos.
4. No más de tres argumentos y en general cuantos menos mejor.
5. Sin daños colaterales. Recuerda, **una cosa**.
6. No uses argumentos '_flag_' (booleanos). Engorronan el código y suelen ser sustituibles por un condicional en el contexto que llama a la función o por un pequeño conjunto de funciones en vez de una sola.

### Comentarios

1. Procura explicarte siempre con el código. Piensa en los comentarios como en un _mal necesario_. Evítalos siembre que puedas.
2. No comentes código. Simplemente elimínalo. Confía en Git.
3. Úsalos para explicar tu intención, clarificar el código o avisar de posibles consecuencias.

### Evita ésto

1. Rigidez. Software difícil de modificar, Un cambio menor causa una cascada de cambios subsecuentes.
2. Fragilidad. Software que se rompe por todas partes modificando una sola.
3. Complejidad innecesaria.
4. Repetición innecesaria.
5. Opacidad. Código difícil de entender.
