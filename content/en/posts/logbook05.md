---
title: "Logbook #05 - A couple of Katas"
url: en/logbook05/
date: 2021-05-09T12:05:06+01:00
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
---

Chill week, let's take a look at a couple of different ways one could approeach these Katas using Java.

<!--more-->

Truth is, [CodeWars](https://www.codewars.com/) is a dangerously fun pastime.
Even doing simple Katas, like the ones below, one can learn something new.
Plus, it seems to me that the fun part is to make a good looking and efficient code, rather than just '_make it work_'.

![logbook](../../../images/ship.gif)

## Kata 1: SqareEveryDigit

#### Objective

Square every digit of a number and concatenate them, return an integer.

#### Example

9119 -> 811181

#### Solution 1

First idea might be to take advantage of the built-in String and Double functionalities from Java to make our life easier:

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

#### Solution 2

We could make it prettier using Java Streams (and while on it, make some more efficient casts/conversions...):

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

#### Solution 3

Or one could just throw everything out of the window, forget about Java and use the power of maths!

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

#### Objective

Given a string of words, return the length of the shortest word(s).

#### Example

"turns out random test cases are easier than writing out basic ones" -> 3

#### Solution 1

The simplest one but also the more efficient regarding execution time (at least on my system):

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

#### Solution 2

More elegant approach using Java Streams:

```java
public static int squareDigits(int n) {

	int min = Arrays.stream(s.split(" "))
			.min(Comparator.comparingInt(String::length))
			.orElse(null).length();

	return min;
}
```

![typing](../../../images/typing.gif)
