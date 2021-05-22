---
title: "Why use a password manager"
url: ./pwdmanager/
date: 2021-03-31T19:49:26+01:00
draft: true
image: /images/pwd.png
categories:
    - why
tags:
    - password manager
    - bitwarden
    - keepassxc
---

Password must be long, can't be repeated and should include caps, numbers, !@#$%^&\* and a dragon scale.
Getting unmanageable yet?

<!--more-->

![crazy](../../../images/crazy.gif)

## It's more comfy than your solution

I can bet that the way you currently manage your passwords is either uncomfortable or insecure. You either have them written in plain text in a file you have to fetch every time you log in (or even worse, written in a physical paper like a **caveman**), or you let your browser manage them for you (good luck changing browser or preventing it's parent company from seeing them).

Good PM, especially if they have a companion browser extension, are literally a one click solution to both creating good passwords and filling them into the login forms.

## You can't guarantee it's not repeated

Even if you think its new and clever, you might very well have used the password you just _'invented'_ for a long forgotten account. Repeating passwords is nearly as bad as setting them to 'admin' or 'password1'.

![repeat](../../../images/repeat.gif)

There is a thing called _dictionary attack_ and it works by spamming leaked or common passwords again a target account. Yours wouldn't stand a chance.

## You can't avoid using a plain text file

And you are probably not encrypting it. We all manage a **huge** amount of accounts, no way you can remember all those passwords. Anybody with access to your machine can access an unencrypted file. Plus you need to be in that specific machine to access your passwords.

## You can't make and remember a single good password

Just look at the requirements for any account password and be honest: Can you really come up with a good one without using personal information like name or DOB? Yeah, me neither.

## Typing huge 14 digit passwords sucks

And you always get something wrong.

## Lost the file? Lost all passwords

If you don't use an external service, your passwords are **gone forever** as soon as that file gets deleted.
Yes, using a third party service is less secure than having a local, encrypted and secured file. More accurately, it depends on how much _trust_ you put in the company behind the service (and yes, ideally trust should be out of the equation).

![no](../../../images/no.gif)

Even so, it would still probably be a more secure (and definitely a more simple) solution.

## Ok, what do I use

Well... a password manager 😀. Here is what to avoid and a personal suggestion.

### Avoid non-foss software at all cost. Here is why:

1. Nobody knows what the code actually does. You are 100% just **trusting** the company offering the service.
2. Foss is always **more secure**. It can be publicly audited.
3. If the company decides to make you pay for the once free features you have no choice, except **maybe** to export a JSON or CSV file.
4. If the company goes six feet under and you react too late, your passwords are gone too.

### Only two reasonable choices:

#### [BitWarden](https://bitwarden.com/)

The more user friendly alternative.
They are widely used and known, are repeatedly audited by third parties, have a free and a paid business service, and have pretty much anything you might need:

-   Desktop GUI
-   Desktop CLI
-   Mobile GUI
-   Browser Plugin
-   Web Vault

Last point is extra important.
You have the option to make an account with them and host your passwords in their servers (just like with any other password manager) **or** you can host your own instance on your own server (using docker) to store and manage your vault and still have access to all their software.

Amazing, free, privacy respecting and no bs.

#### [KeePassXC](https://keepassxc.org/)

Minimal solution (although not as minimalist as just using [pass](https://www.passwordstore.org/)). It's a cross platform implementation of the [KeePass](https://wiki.archlinux.org/index.php/KeePass) standard with added plugin support.

You have a local encrypted vault which you connect to the plugin and that's it.
**You** are in charge of backups and security and can access the vault only locally, but there is literally no one else involved. Not even a connection to your own server.

## Conclusion

I personally use both: BitWarden for convenience (especially on my phone) and KeePassXC for backups and very sensitive information.

![door](../../../images/door.gif)

You wouldn't leave your front door open at night. Be consistent and behave the same way online.
