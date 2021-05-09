---
title: "Logbook #05 - Un par de Katas"
url: es/logbook05/
date: 2021-05-09T12:05:06+01:00
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
---

Semana tranquila, veamos un par de formas de enfrentar estos Katas con Java.

<!--more-->

La verdad es que [CodeWars](https://www.codewars.com/) es un pasatiempo peligrosamente entretenido.
Incluso haciendo Katas simples, como los de abajo, se puede aprender algo nuevo.
Además, parece que la gracia está más en hacerlo bien y bonito que en '_hacer que funcione_'.

![logbook](../../../images/ship.gif)

## Kata 1: SqareEveryDigit

#### Objetivo

Eleva al cuadrado cada dígito del número dado y concatena los resultados. Devuelve int.

#### Ejemplo

9119 -> 811181

#### Solución 1

La primera solución podría pasar por aprovechar las funcionalidades de los String y los Double en Java para facilitarnos el trabajo:

```java
public static int squareDigits(int n) {

	String powered = "";
	var inputAsStringArray = String.valueOf(n).split("");

	for (String s : inputAsStringArray) {
		powered += String.valueOf((int) Math.pow(Double.parseDouble(s), 2));
	}

	return Integer.parseInt(powered);
}
```

#### Solución 2

Podríamos hacerlo más bonito usando Java Streams (y de paso haciendo conversiones de tipos más eficientes...):

```java
public static int squareDigits(int n) {

	return Integer.parseInt(
			String.valueOf(n)
				.chars()
				.map(i -> Integer.parseInt(String.valueOf((char) i)))
				.map(i -> i * i)
				.mapToObj(String::valueOf)
				.collect(Collectors.joining("")));
}
```

#### Solución 3

O también podemos tirar la casa por la ventana, olvidarnos de Java y usar el poder de las mates!

```java
public static int squareDigits(int n) {

	String result = "";

	while (n != 0) {
		int digit = n % 10 ;
		result = digit*digit + result ;
		n /= 10 ;
	}

	return Integer.parseInt(result) ;
}
```

## Kata 2: ShortestWord

#### Objetivo

Dada un String de palabras, devuelve la longitud de la más corta(s).

#### Ejemplo

"turns out random test cases are easier than writing out basic ones" -> 3

#### Solución 1

La más sencilla pero también la más eficiente en cuanto a tiempo de ejecución (al menos en mi sistema):

```java
public static int squareDigits(int n) {

	String[] words = s.split(" ");
	int min = words[0].length();

	for (String word : words) {
		if (word.length() < min)
			min = word.length();
	}

	return min;
}
```

#### Solución 2

Opción más elegante aprovechando Java Streams:

```java
public static int squareDigits(int n) {

	int min = Arrays.stream(s.split(" "))
			.min(Comparator.comparingInt(String::length))
			.orElse(null).length();

	return min;
}
```

![typing](../../../images/typing.gif)
