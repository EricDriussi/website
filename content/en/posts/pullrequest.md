---
title: "How to Pull Request"
url: /en/pullrequest/
date: 2021-04-15T20:22:39+01:00
draft: true
image: /images/prpr.png
categories:
    - howto
tags:
    - github
    - pr
    - pullrequest
    - git
---

Want to contribute to a cool FOSS project you found on Github?

<!--more-->

## Fork 🍴

Forking a Github repo is basically like making a copy of it under your own account. Just click the button on the top right hand side of the page.

![fork](../../../images/fork.jpeg)

Now you have a working copy within your repositories. From now on we'll call this the '_forked repo_' and the original one the '_upstream repo_'.

## Clone

Just run `git clone FORKED-REPO` from the command line to get a local copy of your forked repo.
Now we got something to work with.

## Branch 🌳

Let's keep things tidy, make a new branch and check out to it to keep you work separated.

```bash
git checkout -b pr-branch
```

Go ham on the code!

## Add, Commit & Push

Done coding? Behave like you would when working on your own projects.

```bash
git add .
git commit -m "pull me pls"
git push origin pr-branch
```

<sub>Need to set up [ssh](https://unixmagick.xyz/en/githubssh/)?</sub>

Don't stress, you are just updating **YOUR** fork.

## Create a pull request

![pr](../../../images/pr.png)

## Compare changes

Here you will make sure that the PR is being done **from** the correct repo and branch **to** the correct repo and branch.

![compare](../../../images/comparePR.jpg)

Fill in the message and description below and hit `Create Pull Request`.
You are done!!

## Check

If all went right you should see your PR from the upstream repo.
From here you can also have a back and forth with the upstream dev.

Should there be any amendments to be made, just keep committing and pushing from your local repo to your `pr-branch`. They should all automagically show up in the PR for the upstream dev to check.
