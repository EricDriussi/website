---
url: ./which-are-in/
date: 2021-06-01T18:52:12+01:00
title: "Kata: Which Are In"
draft: true
image: /images/kata.png
categories:
    - howto
tags:
    - kata
    - java
    - string
---

Apparently simple.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [The Problem](https://www.codewars.com/kata/550554fd08b86f84fe000a58/java)

Given two Arrays of Strings '**a**' and '**b**' return a sorted Array '**r**' in lexicographical order of the Strings of '**a**' which are substrings of Strings of '**b**'.

#### Conditions

-   '**r**' must not contain duplicates.
-   '**r**' must not contain null values.
-   '**r**' must be empty if no Strings in '**a**' are substrings of some element in '**b**'.

#### Examples

Example 1:
`a = ["arp", "live", "strong"]`
`b = ["lively", "alive", "harp", "sharp", "armstrong"]`
`r = ["arp", "live", "strong"]`

Example 2:
`a = ["tarp", "mice", "bull"]`
`b = ["lively", "alive", "harp", "sharp", "armstrong"]`
`r = []`

#### Solution

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

## The Process

#### Baby Steps

Let's go for the simplest logic: We'll have to iterate each element of '**a**' and check if it's a substring of some element of '**b**'. If it's the case we'll incorporate it into the result Array.

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

Already in the first line we got a problem: Arrays (at least in Java) are not dynamic. We might set it's length to be equal to '**a**' but it could actually be smaller.
Speaking of which, if there are actually more elements in '**a**' than in '**r**' we'll have null entries.

All without even considering the fact that we still have to order it and exclude duplicates.
There seem to be only headaches down this path.

#### Reinventing the wheel

This thing seems like it would be a lot easier if it we where working with Collections instead of Arrays. Let's give it a shot:

```java
List<String> res = new ArrayList<>();

for (String str1 : a) {
	for (String str2 : b) {
		if (str2.contains(str1) && !(res.contains(str1))) res.add(str1);
	}
}
```

That's a lot better: We got rid of the issue caused by the lack of dynamism of Arrays and now with just a `!(res.contains(str1))` in the `if` we'll ensure theres no null or duplicate entries.

Ordering the ArrayList is as simple as `res.sort(String::compareTo)`.

Now the elephant in the room: We need an Array, not a Collection.

With `res.toArray(T[] array)` we can tell Java to cast the `res` Collection as an Array of the given type.
Since we are invoking the method once the data has been retrieved, we can confidently return the result.
That's how we get the above shown solution.

#### Filtering

Of course, for those with high level Stream-Fu, theres a much more elegant solution:

```java
return Arrays.stream(a)
      .filter(str -> Arrays.stream(b).anyMatch(s -> s.contains(str)))
      .distinct()
      .sorted()
      .toArray(String[]::new);
```

Nearly as simple as writing the instructions in plain English:

-   Filter '**a**' looking for items that coincide as substring as some element in '**b**'.
-   Keep only those distinct or unique (that is, delete duplicates).
-   Order and cast as an Array.

![easy](../../../images/easy.gif)
