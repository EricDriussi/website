---
title: "Kata: Parentesis Válidos"
url: ./valid-parenthesis/
date: 2021-05-12T19:58:03+01:00
draft: true
image: /images/valid.png
categories:
    - howto
tags:
    - kata
    - java
    - string
---

Evita una tarde de sufrimiento.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [El problema](https://www.codewars.com/kata/52774a314c2333f0a7000688/java)

Escribe una función que tome un String por parámetro y devuelva `true` si el orden de los paréntesis es válido.

#### Condiciones

-   El String podrá contener, además de los paréntesis de apertura `(` y cierre `)`, cualquier elemento ASCII válido.
-   El String podrá estar vacío o no contener paréntesis alguno. En tales casos, deberá devolver `true`.
-   Solamente se toman en cuenta los paréntesis simples, es decir, excluimos `([], {}, <>)`.

#### Ejemplos

| String           | Return |     | String          | Return |
| ---------------- | ------ | --- | --------------- | ------ |
| `()`             | true   |     | `)(`            | false  |
| `(())((()())())` | true   |     | `)(()))`        | false  |
| `32423(sgsdg)`   | true   |     | `(`             | false  |
| `adasdasfa`      | true   |     | `())`           | false  |
| ` `              | true   |     | `(dsgdsg))2432` | false  |

#### Solución

```java
public static boolean validParentheses(String parens) {
	String onlyParens = parens.replaceAll("[^()]", "");
	int countParens = 0;

	if (onlyParens.isEmpty())
		return true;

	for (String c : onlyParens.split("")) {
		if (countParens < 0)
			return false;

		countParens = (c.equals(")")) ? countParens - 1 : countParens + 1;
	}
	return countParens == 0;
}
```

Con un mapeo sencillo valdrá incluso si el problema contempla `([], {}, <>)` como paréntesis a considerar.

## El proceso

#### No es recursivo todo lo que reluce

![cat](../../../images/recursing-cat.gif)

Si eres como yo (te gusta torturarte sin motivo aparente) lo primero que habrás pensado es en solucionarlo con una función recursiva.
Al fin y al cabo, para cada paréntesis abierto habrá que comprobar la validez de el substring comprendido entre éste y su respectivo cierre.

Sin embargo para solucionarlo así tendremos que añadir bastante complejidad. Cada llamada a la función deberá contemplar la jerarquía del substring (o en su defecto un stack estático).
En cualquier caso acabaríamos por necesitar otro parámetro, lo cual se sale del tiesto.

#### KISS

Claro que por elegantes (y complejas) que sean las soluciones recursivas, suele haber otra forma de enfocar el problema.
La gracia está en discriminar en qué casos vale la pena el quebradero de cabeza y en cuales no.

Está claro que ante la duda, la solución más simple debería prevalecer.

Si consideramos el problema fríamente y en su forma más simple, se trata en esencia de mantener una cuenta bien llevada de en qué paréntesis estamos.

#### TDD

En suma, el núcleo lógico del problema se soluciona iterando lo siguiente por el String:

`cuenta = (char == ')') ? cuenta - 1 : cuenta + 1`

Claro que ésto no contempla debidamente todos los casos. Para eso está el TDD.
Con los ejemplos expuestos arriba cuesta poco testear cada condición y asegurar la cobertura.

Repitamos las condiciones:

-   El String podrá contener, además de los paréntesis de apertura `(` y cierre `)`, cualquier elemento ASCII válido.

    -   Ya que nos interesa solamente la relación entre los paréntesis, descartemos lo demás:

    `stringLimpia = stringPrincipal.replaceAll("[^()]", "")`

-   El String podrá estar vacío o no contener paréntesis alguno. En tales casos, deberá devolver `true`.

    -   Aquí la traducción a código es casi literal:

    `if (stringLimpia.isEmpty()) return true`

-   `)(` => false

    -   Vamos, que no basta con que el recuento de `(` y `)` sea equivalente. Si se cierra uno antes de abrirse (es decir, si la cuenta es negativa), habrá que parar la máquina y devolver un `false`.

    `if (cuenta < 0) return false`

Refactorizar, usar nombres descriptivos, asegurar el orden de ejecución y listo.

![cool](../../../images/cool.gif)
