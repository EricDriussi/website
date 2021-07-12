---
title: "How to use UFW"
url: ./ufw/
date: 2021-07-12T17:14:37+01:00
draft: true
image: /images/firewall.png
categories:
    - howto
tags:
    - ufw
    - firewall
    - iptables
---

Simple and Uncumplicated!

<!--more-->

## What it is

The Uncomplicated Firewall is just an easy way to interact with `iptables`, the default way for Linux based systems to control connections to and from the internet.

You'll usually see and use it in web servers, although it can --and arguably should-- be installed on your main machine.

## Output

Let's take a look at a pretty standard configuration for a web server:

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed) 
New profiles: skip

To                           Action      From
--                           ------      ----
22                         LIMIT IN    Anywhere                  
80                         ALLOW IN    Anywhere                  
443                        ALLOW IN    Anywhere                  
22/tcp                     LIMIT IN    Anywhere                  
22 (v6)                    LIMIT IN    Anywhere (v6)             
80 (v6)                    ALLOW IN    Anywhere (v6)             
443 (v6)                   ALLOW IN    Anywhere (v6)             
22/tcp (v6)                LIMIT IN    Anywhere (v6)
```

The line starting with `Default` tells us the default policies for all non specified ports. Specific port policies are listed below.

## Setup

If we run `ufw status` right after installing it, we'll get an underwhelming `Status: inactive` as a response.

Makes sense, now let's configure a basic server-ready setup like the one above:

```
ufw default deny incoming # Block everything from the web
ufw limit in 22 # Limit incoming ssh connections
ufw allow in 80 # Allow incoming HTTP connections
ufw allow in 443 # Allow incoming HTTPS connections
ufw enable
```

#### Important: Make sure to not block ssh communication, as that might lock yourself out of your VPS/Server.

Now if you run `ufw status verbose` you should see pretty much the same information as we saw before (formatting may change).

### Deleting rules

For example, if you want to delete the previous HTTPS rule:

```
ufw delete allow in 443
ufw reload
```

### Fine Tuning

Of course, you can easily change the default behaviour as well as fine tune the policy on a per port basis. 
You can `deny`, `reject`, `limit` or `allow` either `in` or `out` traffic for which ever port you might need, as well as use the same parameters to define default behaviours.
