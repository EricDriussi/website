---
title: "Manipular List con Streams: Map & Reduce"
url: ./array-map-reduce/
date: 2021-05-28T16:01:50+01:00
draft: true
image: /images/java-logo.png
categories:
    - howto
tags:
    - java
    - streams
    - map
    - reduce
---

Para quedar guay delante de tus colegas.

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

## Map

También llamado '_Function Mapper_', **devuelve un Stream** con los resultados de haber aplicado la función facilitada a los elementos del Stream entrante.

Dicho de otra manera: `inputStream > Map(miFuncion) > outputStream`.
Donde el `outputStream` es el resultado de aplicar `miFuncion` a `inputStream`.

Dicha función toma la forma de `x -> x.modificacion`.
En la práctica, con el ejemplo anterior:

```Java
List<Integer> list = Arrays.asList(3, 6, 9, 12, 15);

list.stream()
	.map(num -> num * 3)
	.forEach(System.out::println);
```

`Output: 9 18 27 36 45`

También podemos trabajar con métodos desde el map:

```Java
List<String> list = Arrays.asList("esto", "es", "una", "prueba");

list.stream()
	.map(str -> str.length())
	.forEach(System.out::println);
```

`Output: 4 2 4 6`

Ya que estamos con Java 8 o superior, también tenemos acceso a las referencias de métodos (`::`):

```Java
List<String> list = Arrays.asList("esto", "es", "otra", "prueba");

List<String> answer = list.stream()
	.map(String::toUpperCase)
	.collect(Collectors.toList());

System.out.println(answer);
```

`Output: [ESTO, ES, OTRA, PRUEBA]`

O podemos tirar la casa por la ventana!

```Java
List<Persona> personas = Arrays.asList(
		new Persona("Eric", 30),
		new Persona("Jack", 27),
		new Persona("Lawrence", 33)
);

List<Empleado> empleados = personas.stream()
	.map(temp -> {

		Empleado obj = new Empleado();
		obj.setName(temp.getName());
		obj.setAge(temp.getAge());

		if ("Eric".equals(temp.getName())) {
			obj.setSalary(10000);
		}

		return obj;

	})
	.collect(Collectors.toList());

```

Suponiendo las clases `Persona` y `Empleado`, podemos incluso **mapear** un objeto a otro.

## Reduce

Devuelve **un solo resultado** mediante la aplicación repetida de la operación combinatoria (que recibe por parámetro) a los elementos en el Stream que toma por input.

Existen **tres** posibles componentes en ésta operación:

-   **Identidad** (opcional): valor inicial o por defecto, si el resultado es vacío o nulo.
-   **Acumulador**: función que toma dos parámetros:
    -   Resultado parcial de la operación.
    -   Siguiente parámetro a operar.
-   **Combinador** (opcional): función que determina cómo combinar los resultados parciales en caso de ejecución paralela o discordancia entre los tipos del acumulador.

Dicho de otra manera: `inputStream > Reduce(miIdentidad, miAcumulador, miCombinador) > resultado`.

Aparquemos por lo pronto Identidad y Combinador, los veremos más adelante.

`miAcumulador` toma la forma de `(x, y) -> x operación y`, es decir, un lambda.
En la práctica, con un ejemplo:

```Java
String[] array = { "Java", "Streams", "Mola" };

Optional<String> combinado = Arrays.stream(array).reduce((str1, str2) -> str1 + "-" + str2);

if (combinado.isPresent())
	System.out.println(combinado.get());
```

`Output: Java-Streams-Mola`

En principio, un Reduce siempre nos devolverá un `Optional` del tipo que encuentre en el Stream de entrada.
Esto nos obliga a cerciorarnos de que efectivamente existe algún elemento en dicho `Optional` (mediante el `isPresent()`) y luego obtenerlo explícitamente (mediante un `get()`).

Una manera de forzar la obtención de un primitivo es la siguiente:

```Java
int producto = IntStream.range(2, 8)
		 .reduce((num1, num2) -> num1 * num2)
		 .orElse(-1);

System.out.println("El producto es: " + producto);
```

`Output: El producto es: 5040`

Aunque más correcto y extensible sería usar el componente **Identidad** del Reduce.
Al tratarse justamente de la asignación de un valor por defecto en caso de resultado vacío, nos ahorramos el disgusto:

```Java
int producto = IntStream.range(2, 8)
		 .reduce(0, (num1, num2) -> num1 * num2);

System.out.println("El producto es: " + producto);
```

`Output: El producto es: 5040`

En cuanto al **Combinador**, debido a ciertas peculiaridades de la JVM bajo ejecución en paralelo, será necesario para combinar los resultados de los substreams en uno solo.
En el ejemplo se usa una referencia a método como **Combinador**. Como ya sabemos, éstas son sustituibles por lambdas y viceversa desde Java 8.

```Java
int sumaEdades = Arrays.asList(25, 30, 45, 28, 32)
			.parallelStream()
			.reduce(0, (a, b) -> (a + b), Integer::sum);

System.out.println(sumaEdades);
```

`Output: 160`

Cabe mencionar que el **Combinador** será necesario también en caso de usar tipos distintos en el Acumulador.
En el ejemplo, el Acumulador tiene por resultado parcial un `int`, pero por parámetro siguiente un `Usuario`:

```Java
List<Usuario> usuarios = Arrays.asList( new Usuario("Dacil", 30),
					new Usuario("Gabriel", 35));
int result = usuarios.stream()
		.reduce(0, (edadParcial, usuario) -> (edadParcial + user.getAge()), Integer::sum);
```

## Juntos pero no revueltos

```Java
public class ReduceMe {

    public static void main(String[] args) {

        List<Factura> facturas = Arrays.asList(new Factura("01", 5.69f, 2),
						new Factura("02", 10.99f, 15),
						new Factura("03", 14.34f, 10));

        float sum = facturas.stream()
				.map(x -> x.getCantidad() * (x.getPrecio())) // map
				.reduce(0f, Float::sum); // reduce

        System.out.println(sum); // 31.02
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
