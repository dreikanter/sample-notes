---
title: Vim motions I forget and relearn
slug: vim-motions-ref
tags: [vim, reference, tools]
description: A living cheatsheet of vim motions I keep forgetting
public: true
---

# Vim motions I forget and relearn

This is not a comprehensive reference — it's the specific motions I keep looking up and then forgetting again.

**Text objects — the underused ones**

- `ci"` — change inside quotes
- `ca(` — change around parentheses (includes the parens)
- `cit` — change inside HTML/XML tag
- `viB` — visually select inside curly braces
- `dap` — delete around paragraph (including blank lines)

**Jumps**

- `Ctrl-o` / `Ctrl-i` — jump backward/forward in jump list
- `gi` — go to last insert position and enter insert mode
- `gv` — reselect last visual selection
- `'.` — jump to last change
- `''` — jump back to position before last jump

**Marks**

- `ma` — set mark 'a' at cursor
- `` `a `` — jump to exact position of mark 'a'
- `'a` — jump to start of line containing mark 'a'
- Uppercase marks persist across sessions and files

**Search and substitute**

```
:%s/foo/bar/gc   " replace all, confirm each
:5,15s/foo/bar/  " replace in lines 5-15
```

**The thing I forget most**

`=` is the indent operator. `gg=G` re-indents the whole file. `=ap` re-indents the current paragraph. This is faster than any plugin for most cases.

Reference: https://vim.rtorr.com/
