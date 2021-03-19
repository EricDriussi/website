---
url: /en/githubssh/
date: 2021-03-19T20:22:52Z
draft: true
title: "How to use Github with SSH"
image: /images/git-logo.png
categories:
    - howto
tags:
    - github
    - ssh
    - git
---

Github is giving you a headache with its nice _Deprecation Notice_ every time you push? Make it stop!

<!--more-->

## Create SSH keys

The following command creates a pair of keys: a public one (ending in .pub) and a private one.
http://localhost:1313/en/githubssh/#
[We'll copy](http://niceadsl.xyz/en/posts/githubssh/#get-the-public-key-in-the-repote-repo) the content of the .pub file in the github repo's settings.

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

By default it will get saved in `~/.ssh/`, it's recommended to throw in a `mkdir ~/.ssh/github` and store the different keys you'll create in there, giving each pair a relevant name. You'll end up with a key pair for each repo.

#### Important:

You don't need to worry about the `passphrase` IF YOU DON'T CARE ABOUT SECURITY. Otherwise make sure it's at list a short sentence and good luck remembering it!

## Update the local agent

Within the local repo:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github/KEY_NAME
```

## Get the public key in the repote repo

In the github page for the repo:
`Settings > Deploy keys > Add deploy key`

![sample](../../../images/sample.png)
Copy the content of `~/.ssh/github/KEY_NAME.pub` and paste it in the _Key_ field.

## Test your SSH access

Still within the local repo

```bash
ssh -T git@github.com
```

This command will confirm, or deny, that our machine now have access to the remote repo.
If it responds with a _Permission denied_ go back and read carefully.

## Updating repos

If you already had a working repo with HTTPS (the thing they used before SSH), it will keep asking for your username and password.

You'll have to update the origin url:

```bash
git remote set-url origin git@github.com:USERNAME/REPO.git
```

## Cloning

If you was used to cloning repos with HTTPS the command is slightly different.
Well, it's actually just the path format, but you might want to keep it in mind especially if you use the command in your scripts.

```bash
git clone git@github.com:USERNAME/REPO.git
```
