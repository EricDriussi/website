---
url: ./onion/
date: 2021-07-12T17:14:24+01:00
title: "How to get your site on Tor"
draft: true
image: /images/onion.png
categories:
    - howto
tags:
    - website
    - tor
    - onion
---

Why not?

<!--more-->

## Why?

Isn't Tor used by criminals to do bad stuff?
[Kinda](https://2019.www.torproject.org/about/torusers.html.en). It's also used by people that cannot safely browse the clear web and/or express their opinion due to tough restrictions or straight systematic oppression. People that don't want certain queries to be public, journalists, whistleblowers and privacy minded people are known to regularly or occasionally browse the web through Tor.

Of course this alone is not enough to be 100% safe and secure, but practicing good opsec goes far and beyond the scope of this post. Think of it this way: If you think you *might* need to use Tor, you probably **should**.

Plus, it's dead easy so why not make your site accessible for Tor users? Consider that even if you are not using Tor today, you might in the future.

## Install

Since you are probably using a Debian based system, and Tor is not in the standard repos, we'll need to go through the usual hoops:

### Add Tor repos

```
apt install -y apt-transport-https gpg
echo "deb https://deb.torproject.org/torproject.org buster main
deb-src https://deb.torproject.org/torproject.org buster main" > /etc/apt/sources.list.d/tor.list
```

### Add gpg key

```
curl -s https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
```

### Update apt and install Tor

```
apt update
apt install tor deb.torproject.org-keyring
```

## Enable

Now open `/etc/tor/torrc` in your favourite editor, search for the lines
`HiddenServiceDir /var/lib/tor/hidden_service/`
and
`HiddenServicePort 80 127.0.0.1:80`
and uncomment them.

Start the service with `systemctl enable --now tor`.

Get your onion address with `cat /var/lib/tor/hidden_service/hostname`.

## Serve

If you know how to [set up `nginx`](https://unixmagick.xyz/en/nginx/#install--configure) this won't be anything new to you.

Simply create your `nginx` config file for the onion site by launching this into your terminal:

```
nano /etc/nginx/sites-available/your-onion-website
```

Then paste and adjust this config:

```
server {
	listen 80 ;
	root /var/www/onion-site ;
	index index.html ;
	server_name onion-address.onion ;
}
```

Make sure to symlink the config file to `/etc/nginx/sites-enabled` and reload `nginx`

If you find any of this difficult to follow go [here](https://unixmagick.xyz/en/nginx/#install--configure) and come back later.

![yay](../../../images/gremione.gif)
