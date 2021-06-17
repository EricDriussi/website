---
url: ./logbook07/
date: 2021-05-27T20:03:32+01:00
title: "Logbook #07 - Dry DTOs"
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
---

When to repeat code and why avoid it.

<!--more-->

We are told to avoid repetition. What to do with DTOs?

![logbook](../../../images/ship.gif)

## D.R.Y.

Principle by which one should avoid duplicating logic or processes.
Not too different from what we achieve by enclosing a given logic within a function in order to call it elsewhere in the code, in stead of repeating said logic.

Usually attributed to Andrew Hunt and David Thomas in their book "_The Pragmatic Programmer_".

### Process redundancy

**DRY** tells us that, in order to ensure that the least possible actions are taken to achieve a result, there should only be one possible way to realize a task.

### Logic redundancy

To ensure the absence of repeating code (or at least minimize it), we'll resort to abstraction, delegating to each entity **only** it's corresponding logic.

## D.T.O.

It's a type of object used to encapsulate (and transfer, serialize and mutate) data.

The basic idea is that, in a server-client environment, calls from one to the other are expensive.
Using a standard model to send all the needed data, with the basic knowledge needed to interpret it and **nothing more**, we maximize the processes efficiency.

It is worth mentioning that, conversions between domain objects and DTOs (and back) can be expensive, complicated and a bit of a hassle.
What we are really doing by using this pattern is moving complexity from where it's not convenient to us, to where it is.

For that same reason we shall make sure we are doing just that and not **the opposite**.
Here we have a rather interesting rabbit hole. Start [here](https://martinfowler.com/bliki/LocalDTO.html).

## D.R.Y.D.T.O.

![ymca](../../../images/ymca.gif)

So what happens when we get ourselves into the quite common situation of having multiple DTOs with a bunch of duplicated code?

Well, following the DRY principle, we'll make use of abstraction and bada bing bada boom!
Except we just botched it.

Unless the entities corresponding to the DTOs at hand are governed by rather specific and strict abstraction rules, the DTOs shouldn't be either.
In the end they exist to transfer data, not to invent relations between entities.

Even worse, when time comes to modify or delete entities or fields in the future, the last thing we want is to have to unravel a mess of DTOs just to build them back up again.

That's the key: the code might be duplicated **now**, but nothing tells that it will be that way in the **future**.

If the duplicate code within the DTOs can actually be reduced, it's more than likely that the issue comes from further down.
On the other hand, this type of code duplicity can be **casual** and cease to exist within a week.
The DRY principle tells us to avoid duplicating **logic** and structures, not **necessarily** code.

### Conclusion

DRY sure, but well understood.
DTOs sure, but well applied.

![typing](../../../images/typing.gif)
