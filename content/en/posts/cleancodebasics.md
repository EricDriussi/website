---
title: "Clean Code Basics"
url: /en/cleancodebasics/
date: 2021-03-24T14:52:21Z
draft: true
image: /images/uncleBob.jpg
categories:
    - tldr
tags:
    - clean code
    - uncle bob
    - Robert C. Martin
---

<!--more-->

<p align="center">
	This is not the greatest book in the world, no.
	<br>
	This is just a tribute
</p>

![tenaciousd](../../../images/tenacious.gif)

Got the idea from [here](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29), but concepts below come mostly from horizontal reading and Uncle Bob's speeches.

---

Let's set one thing clear: I'm an **awful** reader.

I can listen to podcasts or watch lectures and speeches all day long but I very rarely have the time (or the attention) to actually sit down and read a book.
Only time you'll find me reading is when researching a topic and even that will be mostly horizontal reading.

So just bear that in mind before taking my word for this cornerstone of a book.

---

## Introduction

Before going too deep into the rabbit hole, I'm going to give you an intuitive understanding of what this is all about.

### Beginners

Just try and make your code '_pretty_'.

Look at the indentations, do they make sense? Are there like 5 of them one after another? Are the names you use descriptive? Is there a more efficient way to achieve the same thing? Is your code it elegant?
Do you actually feel proud about it and have an urge to show it to your peers?

### Intermediate

You got your code to work? Great! That means it's machine friendly. Now you need to go back and make it human friendly.

The idea that we should write functioning and clean code at once is nonsense. Humans are not analytical in their thought process, our brains are a big mess when we come up with new ideas (thats kind of the point). First make it work, then clean it up.

> You're not done when it works, You're done when it's right.
> — <cite>Robert C. Martin<cite>

Code is clean if it can be understood easily by a competent reader. Another developer should be able to enhance it (or fix it for that matter).

![wtfdoor](../../../images/wtfdoor.png)

Surprises might be nice irl but I'm not looking forward to going _"WTF?"_ when reading your code. The more predictable the better.

> Clean code does one thing well.
> — <cite>Bjarne Stroustrup <cite>

Just like with Unix philosophy, good software should do **one thing** and do it well.
In this context that _one thing_ is similar to G. Leibniz concept of **Monad**.
To put it simply: if you can't meaningfully refactor your code indo smaller bits, that's one thing.

## Analysis

Let's try and brake it down as much as possible.

![thinking](../../../images/thinking.gif)

### General

1. Conventions are there for a reason. It's easier for me to understand you if we both follow a set of common rules.
2. KISS (Keep it simple stupid). Reduce complexity as much as possible, keep the word _minimalism_ in mind.
3. Always find root cause. Don't just fix the problem, actually fully resolve the issue.

### Understandability

1. Be consistent. If class A has a `var name` and a `var surname`, class B shouldn't have a `var firsName` and a `var lastName` in their place.
2. Use explanatory variables and don't worry about name lengths.
3. Prefer dedicated objects to primitive type. There will be less moving parts.
4. Avoid negative conditionals and be generally careful with conditionals, they get out of hand fast.

### Names

1. They should be descriptive, unambiguous, meaningful, pronounceable and searchable.
2. Don't use `data` or `object` in the name. We already know.

### Functions

1. Small.
2. Do one thing.
3. Use descriptive names, possibly verbs.
4. No more than 3 arguments and generally the fewer the better.
5. Have no side effects. Remember, **one thing**.
6. Don't use flag (bool) arguments. They make a mess and can usually be substituted by a conditional in the calling context or by using a set of functions instead of one.

### Comments

1. Always try to explain yourself in code. Think of comments as a _necessary evil_. Avoid them if possible.
2. Don't comment out code. Just remove. In Git we trust.
3. Do use as explanation of intent, clarification of code or warning of consequences.

### Avoid this

1. Rigidity. The software is difficult to change. A small change causes a cascade of subsequent changes.
2. Fragility. The software breaks in many places due to a single change.
3. Needless Complexity.
4. Needless Repetition.
5. Opacity. The code is hard to understand.
