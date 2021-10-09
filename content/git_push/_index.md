---
title: "Git Push"
date: 2021-10-09T14:40:16-05:00
weight: 0
draft: false
---

I setup git push to my old laptop, which I'm using for testing, with the instructions [here](http://toroid.org/ams/git-website-howto).

As an example, on the remote laptop I did the following:

```
cd MacBookCode
mkdir nGramBare
mkdir nGram
cd nGramBare
git init --bare
GIT_WORK_TREE="~finity/MacBookCode/nGram"
echo '#!/bin/sh'"
GIT_WORK_TREE=$GIT_WORK_TREE git checkout -f" > hooks/post-receive
chmod +x hooks/post-receive
```

On my MacBook I did the following within a directory already managed by git:

```
git remote add finitylaptopcabled ssh://finitylaptopcabled/~finity/MacBookCode/nGramBare/
git push finitylaptopcabled +master
git config branch.master.remote finitylaptopcabled
```

That last line sets the remote laptop up as the default push destination.  Now "git push" on the MacBook sends the updates over to the bare repository on the remote laptop.  The laptop's post-receive hook unpacks those updates into the directory I want.

