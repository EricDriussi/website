---
title: "Kata: Valid Parenthesis"
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

Avoid a painful afternoon.

<!--more-->

![hair](../../../images/turk-hair.gif)

## [The Problem](https://www.codewars.com/kata/52774a314c2333f0a7000688/java)

Write a function that takes a String and returns `true` if the order of the parenthesis is valid.

#### Conditions

-   The String might contain, in addition to opening `(` and closing `)` parenthesis, any valid ASCII character.
-   The String might be empty or not contain any parenthesis. In these cases it should return `true`.
-   Only consider simple parenthesis, exclude brackets and such `([], {}, <>)`.

#### Examples

| String           | Return |     | String          | Return |
| ---------------- | ------ | --- | --------------- | ------ |
| `()`             | true   |     | `)(`            | false  |
| `(())((()())())` | true   |     | `)(()))`        | false  |
| `32423(sgsdg)`   | true   |     | `(`             | false  |
| `adasdasfa`      | true   |     | `())`           | false  |
| ` `              | true   |     | `(dsgdsg))2432` | false  |

#### Solution

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

A simple mapping should make this work even if `([], {}, <>)` should to be considered.

## The process

#### Not all who wonder are recursive

![cat](../../../images/recursing-cat.gif)

If you are anything like me (which is to say, you like torturing yourself for no apparent reason) the first thing that came to mind was solving it with recursion.
Come to think about it, for any given open parenthesis we'll have to check the validity of the substring included within it and its respective closing parenthesis, right?

However, to solve it like that we'll have to add a bunch of complexity. Every function call would have to consider the hierarchy of said substring (or at least some external static stack).
In any case we'll end up needing another parameter, which already makes it not a valid solution.

#### KISS

Truth be told, no matter how elegant (and complex) recursive solutions might be, there's often another way to face the problem.
The interesting part is to be able to discern in which cases is it worth the headache, and in which ones not.

It's clear however that, when in doubt, the simpler solution should prevail.

If we consider the issue in it's most simple form, it's just a matter of maintaining a well thought out count of which parenthesis we are currently in.

#### TDD

Basically, the logical core of the problem might be solved by iterating this through the String:

-   `count = (char == ')') ? count - 1 : count + 1`

Sure, this doesn't consider all given cases, but that's what TDD is for.
With the given examples and rules it's fairly easy to test each condition and assure a proper coverage.

Let's repeat the conditions:

-   The String might contain, in addition to opening `(` and closing `)` parenthesis, any valid ASCII character.

    -   Since we are only interested in the relations between parenthesis, let's discard everything else:

        -   `cleanString = mainString.replaceAll("[^()]", "")`

-   The String might be empty or not contain any parenthesis. In these cases it should return `true`.

    -   Here English-to-Code translation is nearly literal:

        -   `if (cleanString.isEmpty()) return true`

-   `)(` => false

    -   So it's not enough for the count of `(` and `)` to be equivalent. If one closes before opening (or, if the count goes negative), we should stop iterating and return `false`.

        -   `if (count < 0) return false`

Refactor, use descriptive names, double check the execution order and all good.

![cool](../../../images/cool.gif)
