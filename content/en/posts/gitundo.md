---
title: "How to undo using git"
url: ./gitundo/
date: 2021-05-22T17:46:36+01:00
draft: true
image: /images/tardis.png
categories:
    - howto
tags:
    - git
    - github
    - undo
---

Reset, revert and more!

<!--more-->

Git is an awesome piece of software, but it can also be as **complex** as you want it to be.
There are plenty of ways of undoing changes with git, partly because it's a bit like a time machine.

### Get the commit id

We'll need it later:

```
git log --pretty=oneline
```

Its just **vi**, don't worry about it. You can quit with `q`.

## Go back to the last 'add'

### Case

You threw a `git add .` when all was good and you went on coding.
Now you are not liking those 4 `while` Inside one another. How do you go back?

### Solution

If we are talking about changes in one file or directory, a `git checkout -- .` should suffice.
If, on the other hand, the you went in knee deep:

```
git clean -df
git checkout -- .
```

### Explanation

Knowing that Git works on 3 '_levels_' (_Working-dir, Staged o Index y Commited o HEAD_),we are simply getting back to the last Staged, discarding all changes in the Working-dir.

## Go back to the previous commit

### Case

You did a `git commit -m 'cool commit'` a while ago and you find your cat walking on the keyboard. Gotta start all over again ¯\\\_(ツ)\_/¯ .

![cat](../../../images/cat-keyboard.gif)

### Solution

In this case we'll want to get back to the previous stage, nuking all new changes on our way.

```
git reset --hard <id-prev-commit>
```

But, we can also get rid of the last commit by itself, without deleting any new changes.

```
git reset --soft HEAD~1
```

### Explanation

`git reset` undoes all changes after a given commit (or HEAD).
The tags `hard` and `soft` determine if we are trying to undo changes in actual files or just the commits.

## Undoing a merge

### Case

You just merged the `feature` branch with the `develop` branch thinking you were done with the implementation.
Turns out you only read **half** of the ticket.

![facepalm](../../../images/facepalm.gif)

### Solution

Remember that, to git, a merge is just a weird commit.

```
git reset --hard <id-prev-commit>
```

### Explanation

As was the case before, we simply go back to how it all was before the merging commit. We'll use the `hard` tag since it's pretty hard to imagine a case for any other.

## Undo a change (or merge) already pushed

### Case

Thats the classic 'Well that went all righ... **OH GOD NO**'.

![no](../../../images/no.gif)

### Solution

Don't panic!

```
git revert -m 1 <id-commit/merge>
git push -u origin feature/or-what-ever
```

### Explanation

If `git reset` nukes changes and/or commits, `git revert` creates a new one that undoes the changes made by the given commit.
Remember, other people might be working on the **same** `origin` as we are, we shouldn't take the carpet under their feet all willy nilly.

`-m 1` implies that git will preserve the branch to which the merge was done.
If it's not a merge we'll simply not use the `-m` tag.

![prettycool](../../../images/prettygood.gif)
