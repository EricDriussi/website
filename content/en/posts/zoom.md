---
title: "Why Avoid Zoom"
url: ./zoom/
date: 2021-06-06T16:44:07+01:00
draft: true
image: /images/zoom.png
categories:
    - why
tags:
    - zoom
    - privacy
    - security
---

How is this app even legal and why are we still using it?

<!--more-->

## Zoom

Still [the most popular](https://www.digitalinformationworld.com/2021/04/top-video-call-platform-by-market-share.html) video conferencing software worldwide. There's little to no chance you haven't used it.
It has a number of really useful features: remote control, file sharing, voice mail, cloud recordings, annotations, whiteboard, etc.
It's easy to use, reliable and for the vast majority of users it's incredibly convenient.

That's all well and good (although some of those '_features_' should at least raise an eyebrow...) but is the comfort worth the risk?

## Deception

There are quite a few instances where Zoom has deceived their users and the general public.
Let's go over them.

### Security

##### Encryption, or lack there of

It **might** be end-to-end encrypted now, but it certainly wasn't the case back in march 2020.
Every tech company faces issues with security.
However, not every tech company makes bold, [completely false claims](https://theintercept.com/2020/03/31/zoom-meeting-encryption/) about its security in order to sell more product.

##### Yea, just use my Mac as a Server, it's fine...

Apple was forced to (silently) step in to secure millions of Macs after a [security researcher found](https://www.theregister.com/2019/07/09/zoom_mac_webcam_security_patch/) Zoom installed a secret web server on users' Macs, which stayed there after the client was uninstalled.
This meant any malicious website could activate Mac webcam with Zoom installed without the user's permission.

Oh, and in case that's not sus enough: Zoom wanted the researcher to sign a non-disclosure agreement.
Not cool.

##### You might not like Facebook...

![facebook](../../../images/facebook.jpeg)

But Facebook most definitely likes you...
Don't have a Facebook account? No worries, Zoom was quietly [sending data](https://www.vice.com/en/article/k7e599/zoom-ios-app-sends-data-to-facebook-even-if-you-dont-have-a-facebook-account) to Facebook about user habits, even when the user didn't have a Facebook account.

##### Feature...?

It might sound weird but Zoom had a ['_feature_'](https://www.huffpost.com/entry/zoom-tracks-not-paying-attention-video-call_l_5e7b96b5c5b6b7d80959ea96) with which a host could check if participants are clicking away from the main Zoom window during a call.
It would be nice to **not** have you check on my machine constantly, thanks.

##### ZoomBombing

Yep, not too long ago you had to actually watch out for random trolls [streaming porn](https://techcrunch.com/2020/03/17/zoombombing/) on '_private_' video conferences.
That's actually kinda funny.

##### **OUR** data

Remember that lack of encryption from earlier? There technically was encryption going on... Just between their clients and their servers.
Soooo...
Zoom says they [didn't](https://www.consumerreports.org/privacy/zoom-tightens-privacy-policy-says-no-user-videos-analyzed-for-ads/) (big surprise), but they are fully capable of collecting user data (including video stream, audio stream, names, chat... The whole lot) since they keep the encryption keys for themselves.

![ours](../../../images/bugs-bunny.jpeg)

Thanks for that!

##### **OUR** r&d

[Here](https://citizenlab.ca/2020/04/move-fast-roll-your-own-crypto-a-quick-look-at-the-confidentiality-of-zoom-meetings/) you have a quick crash course on why having most of your development in China is maybe not the most trustworthy thing you can do, especially if you want the general public to believe the CCP is not pressuring you.

Sure it costs a fraction of what it would in the EU or the US.
Just don't expect me to trust your company.

##### **OUR** Tiananmen

![this-guy](../../../images/xi-jingpooh.png)

What would you know, talking crap about Winnie the Pooh gets you in trouble.
Who would have thought...
From Hong-Kong and wanting to commemorate the [1989 Tiananmen Square Massacre](https://en.wikipedia.org/wiki/1989_Tiananmen_Square_protests) while on US soil?
![this-guy](../../../images/this-guy.png)

How about a healthy dose of [censorship](https://www.wsj.com/articles/zoom-catches-heat-for-shutting-down-china-focused-rights-groups-account-11591863002?mod=lead_feature_below_a_pos1) instead?
They even admitted to doing it... to ['_comply with local law_'](https://www.axios.com/zoom-closes-chinese-user-account-tiananmen-square-f218fed1-69af-4bdd-aac4-7eaf67f34084.html)...

### Trust

So yeah, trust is basically non existent when it comes to these people.
To be fair, using their service from the browser is significantly more sane and secure.

I mean you can't say it's exactly easy to start your own room from their website, and most of the '_cool features_' are gone.
But frankly speaking, that's exactly why it's more secure. Those aren't reasonable features, they really shouldn't be there.

And yes, they say that most of the issues are gone now. But as always, no one knows.
Closed source software require users to **trust** the company behind it.
Open the source code and the community will decide it's safe or not.

## Alternatives

### Skype, Google Meet, Discord & Teams

Yes, Microsoft and Google are not exactly trustworthy. But I'd rather have a spying company beholden to the US Government than to the CCP.
Plus, you are probably using Windows so... Might as well go all the way.
¯\\\_ (ツ)\_/¯

### [Zipcall](https://www.zipcall.com/)

It works entirely in the browser, both desktop and mobile.
You can create a video conference room with its auto-generated links or key in your own phrase.
You can mute your microphone, pause your webcam, share your screen, or chat in a text pop-up within the app.

Again, not to far from Zoom from the Browser.

### [Team.Video](https://team.video/)

Similar story but with a stupidly pretty and modern UI. Also, lots of useful [features](https://www.notion.so/Team-Video-Gu-a-del-Usuario-27eb22665b454135b2d429702b1911c6) you might be interested in.

### [GoTo Meeting](https://www.gotomeeting.com/)

![goto](../../../images/goto-meeting.png)

This has been around forever and probably has the most amount of conference/meeting ready features of the bunch.

### [Jitsi Meet](https://meet.jit.si/)

Wait, there's a FOSS alternative to an unjustifiably popular proprietary piece of software? Go figure!

![goto](../../../images/jitsi.png)

Pretty much all the bells and whistles but with all the benefits of being FOSS.
No reason to think the people behind are grabbing data but, if you want, you can just host it in your own server.
Amazing!
