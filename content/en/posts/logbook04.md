---
url: /en/logbook04/
date: 2021-04-28T20:02:32+01:00
title: "Logbook #04 - About Descriptive Names"
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
---

Le luxury of programming '_without a clue_'.

<!--more-->

To be honest, [Huella Positiva](https://github.com/ayudadigital/huelladigital-backend) is the first time I see myself involved in a project with these three important features.

-   Open Source.
-   Already started.
-   With quite a bit of already written code.

![logbook](../../../images/ship.gif)

It might seem like a triviality: Talk to the devs, read the code and have at it.
However we'll find a couple of issues:

-   Part of the codebase was written by devs that are **no longer** actively involved.
-   Part of the codebase was written quite a few months ago.
-   Code written by others isn't exactly an **easy** read.
-   Where the hell should I fit **my** code?

Hence the importance of using descriptive names.
If your class or method name allows for a **foreign** dev to get a sense of what's going on, it's a descriptive name.

While the number of classes and methods, as well as the features of the app grows, that '_read the code_' gets more and more complicated.
Global search functions found in most IDEs make it a lot easier to get a basic orientation of where's the issue.

But having the ability to just throw a `Ctrl + Space` and simply read the options provided by the API makes writing code that much more agile.

Having **compound**, **clear** and **explicit** naming for classes and methods gives a rather significant boost to the workflow, especially regarding the first tasks.

Even without knowing full well (or even partially) the code at hand, we'll be able to skip from one intuition to the next and sort of 'guess' where to go.
_IntelliSense_ will snitch us the answer.

![typing](../../../images/typing.gif)
