---
url: ./maintain/
date: 2021-07-12T17:15:12+01:00
title: "How to VPS"
draft: true
image: /images/firewall.png
categories:
    - howto
tags:
    - vps
    - maintenance
    - server
---

Some basic tips.

<!--more-->

## SSH

The idea is, we generate an ssh key on our computer and make our server trust it (not too different from what we did [here](https://unixmagick.xyz/githubssh/)).
A SSH key pair is the fastest, easiest and more secure way of remotely accessing your server.

It will allow us to bypass the root password completely, making access quicker, easily scriptable and brute-force proof.

### Generate the pair

```
ssh-keygen -t rsa -b 4096 -C "your@mail.yes"
```

This command should prompt you for a path in which to store the keys (usually in `~/.ssh/`) as well as a passphrase (which you can leave blank).

You will now see a key pair in the path you selected. Let's get one of them on your server.

### Get the public key on your server

```
ssh-copy-id root@yourdomain.com
```

You'll have to enter your VPS password, after which your server will authorize access to the machine holding the SSH key.

Log out and back in. If it bypasses the password prompt it worked!
If it didn't, check the permissions for both the keys and the `.ssh` directory.

### Disable password logins all together

Not a necessary step but if you want a reason to do it just check the output of `journalctl -xe` on your VPS.
Now that you know what brute force can look like, let's make sure the spammers can **never** make it.

Open `/etc/ssh/sshd_config` and find/set the following lines as shown.

```
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

Of course, reload ssh: `systemctl reload sshd`

##### What if I loose the key?

**Don't. Backups are your friends.**

You should always be able to access your VPS from your providers site. They usually offer some sort of local prompt emulated in a browser window. 

Being local to the machine (or at least functioning as such), this will only be accessible to you after you log in to your VPS provider and won't prompt you for authentication.
Then simply undo the changes above.

## Rsync

You'll often find yourself needing to transfer files to and from your server.
Rsync is probably the easiest way to do it: it's fast, reliable and simple.

Make sure it's installed both on your local machine and on the server, then write (or make an alias for):

```
rsync -rtvzP /path/to/localFile root@yourSite.url:/path/on/the/server
```
<u></u>
This command will run <u>r</u>ecursively (including directories), transfer modification <u>t</u>imes (skips non modified files), <u>v</u>isualize the files being uploaded, compress (<u>z</u>?) files for upload and <u>P</u>ick up where it stopped in case of lost connection.

Of course, to download something from the VPS, just reverse the parameters in the command.

## Cronjobs

There are certain routine tasks that are better left to Cron.
This software will take care of running any command with a given frequency or repetition pattern.

Say, for example that you want to automate updates for your server.
You could run `crontab -e` and insert something like `30 2 * * 0 apt -y update && apt -y upgrade` into the file. Make sure to save before closing the file.

Let's break it down:

```
 .------------------ minute (0 - 59)
 | .--------------- hour (0 - 23)
 | | .------------ day of month (1 - 31)
 | | | .--------- month (1 - 12)
 | | | | .------ day of week (0 - 6)
 | | | | |
 * * * * *
30 2 * * 0 apt -y update && apt -y upgrade
```

Basically a machine friendly version of '*please update the system every Sunday at 2:10AM*'.

There's plenty you can do with cronjobs, and listing examples is beyond the scope of this post.
[This](https://crontab.guru/) website however does a great job at just that!

## Local Searches

Linux file-systems can get a bit chaotic.
You might be used to `whereis` or `which` but that can be insufficient depending on the task at hand.

`updatedb` can quickly index the whole system, which is neat since there's a tool called `locate` that can easily find files and directories whose paths contain any given string.

So if you run `updatedb` and then `locate cron`, you will see a list of files and directories containing `cron` in their paths.
Pretty cool!

## Troubleshooting

Troubleshooting issues is quite common when working on a VPS, and you'll likely find the root cause in the logs.

System wide logs can be seen by running `journalctl -xe` (`-xe` is to make the results a bit more usefull). Of course you can make your life easier if you know what you are after:

```
journalctl -xeu brokenProgram
```

This will only show entries relevant to your `brokenProgram`.

Of course, not all apps use the system logs. These will usually have their own under `/var/log/`. Have a look around, you'll definitely find what you're after.

Another useful troubleshooting utility is `systemctl`. It won't give you any logs, but you can use it to stop, restart, reload and start services manually and/or check their status:

```
systemctl status appNotWorking.service
```

This command will give you plenty of information regarding that specific service.
