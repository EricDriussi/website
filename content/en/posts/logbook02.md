---
title: "Logbook #02 - About names and synonyms"
url: /en/logbook02/
date: 2021-04-15T19:42:03+01:00
draft: true
image: /images/logbook.png
categories:
    - leanmind
    - tldr
tags:
    - leanmind
    - terms
    - synonyms
---

Something strange happens with terms within th IT space.

<!--more-->

Some remain intact (even their pronunciation) not just in English, but in other languages as well. Like it's the case with '_Agile_' (Spanish speaking folk, even knowing full well there's a reasonable and similar translation in Spanish, keep writing and pronouncing it in English).
Others instead seem to dance between clarity and indecision. 20 synonyms, who gives more?

![logbook](../../../images/ship.gif)

If you reed '_CPU_' you immediately know what I'm talking about. Doesn't matter if it's the Intel 8086 of the latest Threadripper, we all understand that it's a processor.
We know what an '_Array_' is, that it's an aggregate of elements and a type of collection. Nobody would try calling it _Lectern_ in Python and _Replenisher_ in Rust.

Not the same with functions, which in OOP languages like Java are called **methodes**.
Are there subtle differences? Sure.
Are those differences mostly irrelevant and both basically the same logical structures? Yup.

### Hexagonal Ports

It's even better in the case of (how some people like to call it) _Hexagonal Architecture_ or (how others like to call it) _Ports and Adapters_.

![hexagonal](../../../images/hexagonal.png)

We might ask what exactly does a hexagon have in common with a ports and adapters system (¯\\\_ (ツ)\_/¯).

Of course, in case that wasn't confusing enough, where Ports and Adapters have three layers: Core, Ports, and ... Adapters, our friendly hexagon has Domain, Application, and Infrastructure, respectively.

We could try and clarify if we are talking about a web Domain or Domain as in scope, if the Infrastructure es (how one might expect) that which does **not** change or ... The exact opposite. If it makes any sense to call the part and the whole by the **same name**.
'The Application must be down due to and issue in the Application layer' doesn't sound exactly ... Helpful.

![ports&adapters](../../../images/portsadapters.png) <sub>source: https://www.youtube.com/watch?v=ttUDqYgBvRU </sub>

It's clear which of the terms is used as '_marketing_' and which one is actually useful.

### BDD

Another peculiar one is Behaviour Driven Development.
Not that it's a bad name, but it does hint at a vague '_mindset_' when it comes to organizing a proyect. It doesn't suggest an specific operating procedure or a concrete methodology.

There's a sentence that caught my attention when reading about this:

> He [Dan North] called this concept Behaviour Driven Development (BDD), intentionally removing the word “test” to encourage business people to remain engaged throughout the process.
> — <cite>The BDD Books - Discovery</cite>

Sure, part of the idea behind BDD is to stress the importance of collaboration between professionals from different fields. Even so, it really does sound a bit '**clickbaity**' to title something to maximize visibility or adoption rather than to represent correctly it's content.

On the other side, at least they didn't call it [cucumber](https://cucumber.io/)...

### Conclusión

One thing is the usual polysemy within a given language but **forcing** concepts in words they just don't fit in is a whole other beast.

Being for _marketing_ or for the current political wind, there seems to always be a considerable majority more than happy to give the latter a try.
One can only wish they might keep in mind that technical languages are already complicated by necessity, and that they are really **not** helping noobs with all their semantics and unnecessary creativities.

![typing](../../../images/typing.gif)

Thank god that fad of naming territories, inventions, discoveries and anatomical structures by the surname of the relevant narcissist is long gone.
