---
title: "Kata: Palabras Contenidas"
url: ./which-are-in/
date: 2021-06-01T18:52:23+01:00
draft: true
image: /images/kata.png
categories:
    - howto
tags:
    - kata
    - java
    - string
---

Aparentemente simple.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [El problema](https://www.codewars.com/kata/550554fd08b86f84fe000a58/java)

Crea un método que, dados dos Arrays de Strings (**a** y **b**) tomados por parámetro, devuelva un Array ordenado (**r**) de los Strings de '**a**' que sean substrings de algún String de '**b**'.

#### Condiciones

-   '**r**' no debe contener duplicados.
-   '**r**' no debe contener nulos.
-   '**r**' debe estar vacío si no hay Strings en '**a**' que sean substrings de algún String de '**b**'.

#### Ejemplos

Ejemplo 1:
`a = ["arp", "live", "strong"]`
`b = ["lively", "alive", "harp", "sharp", "armstrong"]`
`r = ["arp", "live", "strong"]`

Ejemplo 2:
`a = ["tarp", "mice", "bull"]`
`b = ["lively", "alive", "harp", "sharp", "armstrong"]`
`r = []`

#### Solución

```java
public static String[] whichAreInArray(String[] array1, String[] array2) {

        List<String> res = new ArrayList<>();

        for (String str1: array1) {
            for (String str2: array2) {
                if (str2.contains(str1) && !(res.contains(str1))) res.add(str1);
            }
        }

        res.sort(String::compareTo);

        return res.toArray(String[]::new);

}
```

## El proceso

#### Primeros pasos

Usemos la lógica más básica: Habrá que iterar cada elemento de '**a**' y comprobar si es substring de algún elemento de '**b**'. Si es así lo incorporaremos a un Array resultado.

```java
String[]r = new String[a.length];

for (int i = 0; i < b.length; i++) {
	for (int j = 0; j < a.length; j++) {
		if (b[i].contains(a[j])) {
			r[j] = a[j];
		}
	}
}
// ...
```

Claro, ya en la primera línea hay un problema importante: Los Arrays (al menos en Java) no son dinámicos. Lo podremos poner el tamaño de '**a**' pero lo mismo podría medir menos.
Hablando de lo cual, si efectivamente hay más Strings en '**a**' de las que acaban en '**r**' tendremos entradas nulas.

Todo ésto sin contar con que aún tenemos que ordenarlo y excluir duplicados, claro.
Por aquí parece que no hay más que dolores de cabeza.

#### Reinventando la rueda

La situación huele a que sería más fácil si en vez de Arrays fueran Colecciones. Probemos:

```java
List<String> res = new ArrayList<>();

for (String str1 : a) {
	for (String str2 : b) {
		if (str2.contains(str1) && !(res.contains(str1))) res.add(str1);
	}
}
```

Esto ya pinta mejor: Nos ahorramos el problema de la falta de dinamismo y ahora con añadir un `!(res.contains(str1))` al `if` aseguramos que no haya duplicados ni entradas nulas.

Ordenar el ArrayList será tan simple como `res.sort(String::compareTo)`.

Ahora lo evidente: Necesitamos devolver un Array, no una Colección.

Con `res.toArray(T[] array)` podemos decirle a Java que nos convierta la colección `res` a un array del tipo que le pasemos por parámetro.
Al invocarlo una vez obtenido los datos deseados, podemos devolverlo con toda confianza.
Así obtenemos la solución expuesta arriba.

### Filtrado

Claro que para los que ya tenga cierto nivel de Stream-Fu hay una solución mucho más elegante:

```java
return Arrays.stream(a)
      .filter(str -> Arrays.stream(b).anyMatch(s -> s.contains(str)))
      .distinct()
      .sorted()
      .toArray(String[]::new);
```

Casi como escribir las órdenes en inglés:

-   Filtramos '**a**' en busca de items que coincidan como substring de algún item de '**b**'.
-   Mantenemos sólo aquellos que sean distintos o únicos (es decir, eliminamos duplicados).
-   Ordenamos y convertimos a Array.

![easy](../../../images/easy.gif)
