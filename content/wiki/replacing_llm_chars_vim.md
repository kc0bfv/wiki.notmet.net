---
title: "Replacing Common LLM Characters in VIM"
date: 2026-03-17T16:24:00-04:00
---

When I'm working with LLMs these days they're always using the pretty quotation marks, apostrophes, and dashes.  It's no big deal but I don't like it - especially if I'm editing something enough that it becomes *my* output.  I don't want it to be something that I would not have typed.  Here's a one liner for VIM that replaces those things.

```vim
:%s/“/"/g | %s/”/"/g | %s/’/'/g | %s/—/-/g
```
