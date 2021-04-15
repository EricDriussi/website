---
title: "Bitácora #02 - Sobre nombres y sinónimos"
url: /es/logbook02/
date: 2021-04-15T17:59:31+01:00
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
    - terminos
    - sinonimos
---

Algo raro pasa con los palabros en el ámbito de la informática.

<!--more-->

Algunos permanecen intactos (incluso su pronunciación) no solo en inglés sino en otros idiomas, como es el caso de '_Agile_' (aún sabiendo que significa ágil, los hispano-hablantes se **empeñan** en pronunciarlo /ˈædʒaɪl/, como en inglés).
Otros en cambio, parecen bailar entre la claridad y la indecisión. 20 sinónimos por cabeza y quien más tenga más ponga.

![logbook](../../../images/ship.gif)

Si lees '_CPU_' sabes inmediatamente de qué se está hablando. Da lo mismo que sea el 8086 de Intel o el Threadripper de turno, entendemos que se trata de un procesador.
Sabemos lo que es un _Array_, que es un conjunto de elementos y un tipo de colección. A nadie se le ocurriría llamarlo _Atril_ en Python y _Reponedor_ en Rust.

No ocurre igual con las funciones, que en lenguajes OO como Java, se llaman **métodos**.
¿Que guardan sutiles diferencias? Seguramente.
¿Que son mayormente irrelevantes y son en práctica la misma estructura lógica? Pues también.

### Puertos Hexagonales

Es aún mejor el caso de la _Arquitectura Hexagonal_ (como les gusta llamarla a algunos) o _Ports and Adapters_ (que les gusta más a otros).

![hexagonal](../../../images/hexagonal.png)

Habrá que preguntar qué tienen en común un hexágono con un sistema de puertos y drivers (¯\\\_ (ツ)\_/¯).

Claro que, por si fuera poca confusión, donde Ports and Adapters tiene las capas de Core, Ports y ... Adapters, nuestro amigo hexagonal tiene Dominio, Aplicación e Infraestructura, respectivamente.

Sería cuestión de esclarecer si el Dominio es web o sinónimo de 'ámbito', si la Infraestructura es (como cabe esperar) aquello que **no** cambia o ... Justamente lo contrario. Si tiene o no sentido llamar a la parte y al conjunto con el **mismo nombre**.
'La aplicación debe de estar fallando al nivel de la aplicación' suena digamos... _Poco claro_.

![ports&adapters](../../../images/portsadapters.png) <sub>source: https://www.youtube.com/watch?v=ttUDqYgBvRU </sub>

Aquí queda claro cuál es el término de '_marketing_' y cual es el técnicamente útil.

### BDD

Otro caso peculiar es el del Behaviour Driven Development.
No porque sea un mal nombre, sino porque da a entender un vago '_mindset_' a la hora de organizar un proyecto. No es un término que sugiera un proceso operativo concreto o una metodología específica.

Hay además una frase que llamó mucho mi atención al leer sobre el tema:

> He [Dan North] called this concept Behaviour Driven Development (BDD), intentionally removing the word “test” to encourage business people to remain engaged throughout the process.
> — <cite>The BDD Books - Discovery</cite>

Claro que parte de la idea del BDD es la colaboración entre profesionales de distintos ámbitos. Pero no deja de recordar un poco al '**clickbait**' eso de titular algo para maximizar su visibilidad o adopción, más que para representar correctamente su contenido.

Por otra parte supongo que al menos no lo han llamado [pepino](https://cucumber.io/)...

### Conclusión

Una cosa es la normal y **emergente** polisemia del lenguaje (que en idiomas como el español, ya es bastante compleja) y otra bien distinta es **forzar** conceptos en palabras en las que no caben.

Ya sea por _marketing_ o por el viento político de turno, siempre hay una considerable mayoría más que dispuesta a éso último.
Ojalá tuvieran en cuenta que el lenguaje técnico ya es complicado por necesidad y que hacen flaco favor a los novatos con su gala de semánticas y creatividades innecesarias.

![typing](../../../images/typing.gif)

Menos mal que ya pasó la moda de nombrar territorios, invenciones, descubrimientos y estructuras anatómicas con el apellido del ególatra de turno.
