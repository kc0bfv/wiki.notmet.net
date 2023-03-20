---
title: "Tiddlywiki Configuration"
date: 2023-03-20T10:40:00-05:00
draft: false
---

# Adding a Table of Contents

Tiddlywiki has this documented well: https://tiddlywiki.com/static/Adding%2520a%2520table%2520of%2520contents%2520to%2520the%2520sidebar.html

To modify it a little:

1. Create a tiddler called `Contents`
2. Give it the tag `$:/tags/SideBar`
3. Set the text to

```
<div class="tc-table-of-contents">
<<toc-selective-expandable 'Contents'>>
</div>
```

4. Add a `caption` field with the text `Contents`
5. Add a `list-before` field with the text `$:/core/ui/SideBar/Open`