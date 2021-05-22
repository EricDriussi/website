---
title: "How to not use Google"
url: ./web-search/
date: 2021-04-25T11:42:43+01:00
draft: true
image: /images/magnify.png
categories:
    - howto
tags:
    - google
    - searx
    - duckduckgo
    - search engine
---

So you've realized that using Google's search engine is more or less like living in Huxley's '_Brave New World_'. Now what?

<!--more-->

![coyote](../../../images/coyote.gif)

Honestly I didn't leave Google behind for any sort of profound reason or privacy concern. I just found it wasn't up for the task.
Way to much **propaganda**, way to many **ads** and way to many **irrelevant** results.

## Life beyond Google

Contrary to what you might have heard, there are quite a few search engines available to you.
And no, they are not meme-engines. They just don't pretend to be good for everything (while only being good as **spyware**).
Let's break them down:

### [Duckduckgo](https://duckduckgo.com)

Probably the most popular. Closed source back-end but pretty transparent as a company. American based, no tracking, no profiling, no BS.
Pretty good general purpose alternative, except maybe for image search.

### [Startpage](https://startpage.com)

A different front-end to Google's back end. The idea is that you still want results from Google (for some reason) but would rather not have the NSA over for dinner.
This method [should](https://github.com/prism-break/prism-break/issues/168) remove targeted crap.
They are based in Europe which might give you some peace of mind.
Still, closed source so you'll have to trust them and Google (to a minor extent).

### [Swisscows](https://swisscows.com)

Super family friendly SE. Built-in blockage of sex, porn, violence and the likes.
It tends to use Bing under the hood but also does it's own indexing.
Same as the above, claims to be privacy respecting (and seems like it), but being closed source you'll have to trust the Swiss if that's your concern.

### [Bing](https://bing.com)

Yes, trusting Microsoft instead of Google is hardly any better. But, odd as it sounds, nowadays Microsoft is kind of the 'small' guy in pretty much everything but OS usage.
Does it track the life out of you? Most definitely, but it's still preferable to have 5 different companies partially tracking you than to have one knowing you better than you know yourself.
It's still useful for getting a different perspective (you might have noticed some... Let's call them **biases**, on Google's results).
Also, it's actually pretty good for image searches.

### [Yahoo](https://yahoo.com)

The same reasoning as above more or less applies here as well. Still a shitty service privacy wise, but useful if understood correctly.
Plus, if you are into crypto or finance in general, it has some pretty useful tools.

### [Yandex](https://yandex.com)

For those who hate the NSA but would love to meet the ~~KGB~~ SVR. Based in the Netherlands but also kind of Russian, it seems really sketchy but might be interesting for political research. You can bet you wont find much American **propaganda** there.
It's also very widely used in the Russian general sphere of influence.
Still wouldn't use it w/o a proxy or VPN.

## Search with Searx

'_So what now? Am I supposed to use six search engines instead of one?_'

![yesbutno](../../../images/yesbutno.jpg)

You can just use a **meta SE**, like [Searx](https://searx.unixmagick.xyz/)!
Simply put, it queries a bunch of different SE for you and presents all the results in a single page.

![meta-search](../../../images/meta-search.png)

-   It doesn't offer personalised results, because it doesn't generate a profile about you.
-   It doesn't save or share what you search for.
-   It's fully open source (code [here](https://github.com/searx)) so you can actually host your own instance (like I do with the one linked above) or just chose one you trust from [this](https://searx.space/) list and use it.

If you want you can set exactly how and which SE it queries. If you don't, you can just go ahead and use a public instance as is.
It wont work with every SE available but it probably offers you more than you need, and it's sort of the price to pay for convenience.
