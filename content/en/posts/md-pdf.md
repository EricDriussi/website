---
title: "The reason you can't render your books like LeanPub does"
url: ./md-pdf/
date: 2021-07-14T12:31:26+01:00
draft: true
image: /images/md.png
categories:
    - why
tags:
    - markdown
    - leanpub
    - pdf
    - pandoc
---

Short answer: Because there's no standard markdown and probably never will be.

<!--more-->

## LeanPub

<img style="float: right; margin: 3%" src="../../../images/leanpub.png">

First and foremost: LeanPub is both an online shop and a writing plus publication platform.
It's based on the idea of [Lean Publishing](https://leanpub.com/manifesto) according to which books are published '*unfinished*' using agile tools and iterated over considering the reader's feedback, updating the publication whenever the topic at hand requires it.

If any of this sounds familiar it's because it stems from the Agile philosophy, which makes it very enticing to publishers and/or authors related to the IT space.

### Format

Knowing the kind of people that use this service the most, it shouldn't surprise you that the author is expected to write in Markdown and that's then used to generate PDF, ePub and mobi (formats whith which the book is presented to the public).

Specifically they use '*Lean Flavoured Markdown*' and more recently [*Markua*](http://markua.com/) ([not quite the same](http://help.leanpub.com/en/articles/2030628-where-are-some-simple-sample-leanpub-flavoured-markdown-or-markua-books), but not too different), they even have a [manual](https://leanpub.com/markua/read#leanpub-auto-differences-with-leanpub-flavoured-markdown
) for this last one.

### Problems... or not

It's always useful to understand the tool being used. If you are having issues and getting frustrated with LeanPub as an author, you might want to reconsider [what's the aim of this service](https://leanpub.medium.com/why-dont-i-use-leanpub-4ad2a77f5744).

That being clear, there are some valid criticisms floating around the web:

- In some cases it might be excessively slow, especially given that you might have to render the book over and over to ensure adequate formatting.
- There are some [bugs](https://github.com/thephpleague/commonmark/issues/97) that can get somewhat tiresome.
- It's a remote process. This means that the authors (often developers) can't modify the behaviour nor solve issues on their own
- They have an [API](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html) but it's exclusive for Pro accounts.
- As you might have guessed, it ain't [FOSS](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html).

So, what alternatives do we have?

## All roads lead to Pandoc

![nginx](../../../images/pandoc.png)

After not much research, it's clear that [Pandoc](https://github.com/jgm/pandoc) is the center of all file conversion to and from [all sorts of formats](https://pandoc.org/).

It has quite a few wrappers for converting between all kinds of file types simply by running a well thought out command:

`pandoc -f INPUT-FORMAT -t OUTPUT-FORMAT INPUT-FILE -o OUTPUT-FILE`

It's a very comfy and fundamental tool for workflows that rely heavily on document management.

Not only that, the vast majority of tools and services that offer file conversion capabilities are simply Pandoc with a fresh coat of paint: At a low level they rely on this software to offer their services.

## Markdown

It might seem a little weird at first, but it's hard to think of a more comfortable language to compose text with.

The issue is, everyone seems to implement Markdown [however they please](https://en.wikipedia.org/wiki/Markdown#Variants) and unfortunately the implementations we are concerned with aren't exactly widely used, so we can't count on Pandoc coming to the rescue. At least not directly.

Luckily it turns out that the variations used by LeanPub are direct supersets of Commonmark, a much more widely used format with its own Pandoc wrappers. With any luck this command will get you close to what you're after.

```
pandoc -f commonmark_x -t pdf yourBook.md -o yourBook.pdf
```

If it doesn't you might have to resort to `sed` to basically convert the given markdown to a more comprehensible format for Pandoc to process. Although even this might not be enough.
Keep in mind that Markua was created specifically with book drafting in mind and to avoid the need for further processing. It's not certain you'll find another Markdown flavour that's good enough for your use case.

### Where's my standard?

<img style="float: left; margin: 3%" src="../../../images/md.png">

If you made it down here you might be wondering why there's no actual Markdown standard.

The closest thin we have is Commonmark, and it's actually been widely adopted.
However Markdown is a language born to make life easier for whoever is using it (not for a machine) and not everyone using it will necessarily have the same needs.

The purpose of the language is to be lax enough to [adapt](https://twitter.com/gruber/status/507670720886091776) to the requirements of any given circumstance.

A rigid syntax would, at the same time, be of little use for a majority of cases, and defeat the purpose of Markdown all together.
