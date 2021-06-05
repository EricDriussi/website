---
title: "Manipular List con Streams: Filter & Find"
url: ./array-filter-find/
date: 2021-06-04T18:05:57+01:00
draft: true
image: /images/java-logo.png
categories:
    - howto
tags:
    - java
    - streams
    - filter
    - find
---

Para quedar (aún más) guay delante de tus colegas.

<!--more-->

## Java Streams

![stream](../../../images/stream.gif)

Cuándo hablamos de Streams en Java, nos referimos a la API introducida en Java 8 para el procesado de colecciones de objetos.
Es un sistema que soporta la concatenación de varios métodos para producir el resultado deseado.

-   No se trata de una estructura de datos, aunque recibe input de éstas.
-   No modifican el estado del input original, sólo producen un resultado acorde a los métodos usados.
-   Cada paso (operación, método, ...) intermedio devuelve otro Stream, lo cual permite encadenarlos.
-   Los métodos terminales ponen fin al Stream y devuelven un resultado.
-   Ejemplos de operadores intermedios: map, filter, sorted.
-   Ejemplos de operadores terminales: collect, foreach, reduce.

Ejemplo:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);
list.stream().empiezaLaFiesta();
```

## Filter

**Devuelve un Stream** con los resultados de haber aplicado el filtro facilitado a los elementos del Stream entrante.

Dicho de otra manera: `inputStream > Filter(miFiltro) > outputStream`.
Donde el `outputStream` es el resultado de aplicar `miFiltro` a `inputStream`.

Dicha función toma la forma de `x -> x.filtro`.
En la práctica, con el ejemplo anterior:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);
list.stream()
	.filter(num -> num > 5)
	.forEach(System.out::println);
```

`Output: 9 12 15`

También podemos trabajar con métodos desde el filter:

```Java
List<String> list = Arrays.asList("esto", "es", "una", "prueba");

list.stream()
	.filter(str -> str.length() > 3)
	.forEach(System.out::println);
```

`Output: esto prueba`

Además existe la posibilidad de filtrar con más de un criterio:

```Java
List<String> list = Arrays.asList("esto", "es", "otra", "prueba");

List<String> answer = list.stream()
	.filter(str -> str.length() > 3 && str.startsWith("p"))
	.collect(Collectors.toList());

System.out.println(answer);
```

`Output: [prueba]`

## Find

Devuelve **un solo resultado** de entre los elementos del Stream que toma por input.

Existen en Java **dos** tipos de Find:

-   **findFirst**: devuelve explícitamente el primer elemento del Stream, siempre que haya un ordenamiento implícito y de no haberlo actúa como **findAny**.
    Según la documentación los Streams pueden tener un ordenamiento implícito o no, dependiendo de el origen y de las demás operaciones intermedias.

-   **findAny**: devuelve un elemento del Stream sin importar el orden.

    -   **En una operación síncrona, probablemente devuelva el primer elemento del Stream, pero no hay garantías de que lo haga.**

Dicho de otra manera: `inputStream > Find > resultado`.

Ya que `findFirst` es más exacto que `findAny` y actúa de manera predecible en procesos asíncronos, lo usaremos como ejemplo en adelante.

```Java
String[] array = { "Streams", "Java", "Mola" };

Optional<String> combinado = Arrays.stream(array).sorted().findFirst();

if (combinado.isPresent())
System.out.println(combinado.get());
```

`Output: Java`

En principio, `findFirst` nos devolverá siempre un `Optional` del tipo que encuentre en el Stream de entrada.
Esto nos obliga a cerciorarnos de que efectivamente existe algún elemento en dicho `Optional` (mediante el `isPresent()`) y luego obtenerlo explícitamente (mediante un `get()`).

Una manera de forzar la obtención de un primitivo es la siguiente:

```Java
int first = IntStream.range(2, 8)
	.findFirst()
	.orElse(-1);

System.out.println("El menor es: " + first);
```

`Output: El menor es: 2`

## Juntos pero no revueltos

```Java
public class ReduceMe {

    public static void main(String[] args) {

        List<Factura> facturas = Arrays.asList(new Factura("01", 5.69f, 2),
						new Factura("02", 10.99f, 15),
						new Factura("03", 14.34f, 10));

        Factura facturaMinimaCantidadMayorACinco = facturas.stream()
                .filter(x -> x.getCantidad() > 5) // filter
                .sorted((facturaMayorCantidad, facturaMenorCantidad) ->
                                (facturaMenorCantidad.getCantidad() < facturaMayorCantidad.getCantidad()) ? 1 : -1)
                .findFirst() // find
                .orElse(new Factura("00", 0f, -1));

        System.out.println(facturaMinimaCantidadMayorACinco.cantidad); // 10
	}
}

class Factura {

    String num;
    float precio;
    int cantidad;

    public Factura(String num, float precio, int cantidad) {
        this.num = num;
        this.precio = precio;
        this.cantidad = cantidad;
    }

    public float getPrecio() {
        return precio;
    }

    public int getCantidad() {
        return cantidad;
    }
}

```
