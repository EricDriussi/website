---
title: "How to serve your website with Nginx"
url: ./nginx/
date: 2021-07-12T11:50:30+01:00
draft: true
image: /images/nginx-logo.png
categories:
    - howto
tags:
    - website
    - nginx
    - vps
---

It's easier than you might think!

<!--more-->

I'm going to assume that you already have either a physical local server or a VPS with root access, as well as a registered domain name.

## Connect to your server

<img style="float: left; margin: 3%" src="../../../images/ssh.png">

```
ssh root@yourwebsite.org
```

This command will ask you for your password, just type it in and you'll log into your server/VPS.

If you get an error here it might be one of three issues:
- **Incorrect DNS Settings**: check your registrar and in the meantime, use your VPS IP instead of your domain.
- **DNS record not yet propagated**: you might have to wait a while for the DNS records to update. Just as above, use the IP in the meantime.
- **Old SSH host**: You might have and old ssh connection with that URL (if you nuked your server or are reusing an old URL). 

	Throw a `cp .ssh/known_hosts .ssh/known_hosts.old`, just in case.
	Check in `.ssh/known_hosts` for one or more lines that start with your domain and delete them.

## Install & Configure 

![nginx](../../../images/nginx.png)

Assuming you are running a Debian based system:

```
apt update
apt upgrade
apt install nginx
```

Nginx is a webserver. You can tell it where your site is and how to host it on the internet. 
How it works is by having a `sites-available` directory that holds the config for how nginx should serve any given site, and symlinking that config file to `sites-enabled`.

So, let's begin!

### Config

Let's begint by creating said config file:
```
nano /etc/nginx/sites-available/yourwebsite
```
Of course, name `yourwebsite` whatever you like.

Just copy the default config from below:

```
server {
	listen 80 ;
	listen [::]:80 ;
	server_name yourwebsite.org ;
	root /var/www/yourwebsite ;
	index index.html index.htm index.nginx-debian.html ;
	location / {
		try_files $uri $uri/ =404 ;
	}
}
```

And again, name `yourwebsite` whatever you want, or rather whatever your **domain name** is.


##### What's going on?

<img style="float: right; margin: 2%" src="../../../images/wtf.gif">

- **listen**: Tells nginx to listen for port 80 on both IPv4 and IPv6.
- **server_name**: When someone connects to this server and looks for that address, they will be directed to what we point `root` at.
- **root**: As explained above, this is the path for your website within the server. It can be whatever you want, but it's kinda standard to put websites in `/var/www/`.
- **index**: Just the default starting point for your website.
- **location**: Tells the server how to look up files, and otherwise throws a 404 error.

Once this is taken care of --and you've created your site in the specified directory--...

## Enable the site

Use `ln -s` to symlink the config file as explained above and reload (or restart) nginx to make it load the new configurations:

```
ln -s /etc/nginx/sites-available/yourwebsite /etc/nginx/sites-enabled
systemctl reload nginx
```

This **should** give you a functioning (although not secure) website.

Chances are, your VPS came with strict firewall settings by default to prevent you from doing something stupid.

## Open up the Firewall

Hopefully your VPS came with `ufw` pre-installed. If not, make sure to [install, start and enable it](https://unixmagick.xyz/en/ufw/) yourself. In any case you'll need to launch the following commands:

```
ufw allow 80
ufw allow 443
ufw limit 22
```

These commands allow/limit incoming traffic to ports 80 (http), 443 (https) and 22 (SSH).
We limit port 22 to slow down the hoards of spambots that constantly try to brute-force into any and all public facing web servers.

#### Important: Make sure to not block ssh communication, as that might lock yourself out of your VPS/Server.

This should allow your website to be reachable from the web although **still in a non secure** fashion. Speaking of which, let's take advantage of that 443 port we opened and fix this.

## Be the S in HTTPS

<img style="float: left; margin: 2%" src="../../../images/lock.png">

There are multiple wasy of going about this, but by far the easiest is to use `certbot`.

```
apt install python-certbot-nginx
certbot --nginx
```

Just follow the instructions, it's dead easy. 
It'll ask for your email, for which domain to certify and if you want to redirect traffic from HTTP to HTTPS. This is definitely the way to go, so feel free to select option number 2 here.

### Bonus

Certificates created with this method need to be renewed every three months.
This is why it asks for an email address: to notify the user of expiring certs.

Automate this using cron!

```
crontab -e
```

Paste `0 0 2 * * certbot --nginx renew` into the file to create a cronjob that automatically asks `certbot` to renew all certs, and does so every two months.
