---
title: Vim motions — daily reference
slug: vim-motions-reference
tags: [vim, reference, tools]
description: Motions and operators I use daily and ones I keep forgetting.
public: true
---

# Vim motions — daily reference

This is a reference for my own practice. Not a tutorial. Official docs: https://vimdoc.sourceforge.net/htmldoc/motion.html

## Text objects (most useful thing in Vim)

```
ciw   change inner word
caw   change a word (includes surrounding space)
ci"   change inside double quotes
ca"   change a double-quoted string including quotes
cit   change inside tag (HTML)
ci(   change inside parens
ci{   change inside curly braces
```

Same pattern with `d` (delete), `y` (yank), `v` (visual select).

## Jumps

```
%     jump to matching bracket/paren/brace
*     search forward for word under cursor
#     search backward for word under cursor
gd    go to local definition
gD    go to global definition
''    jump back to where you came from (after a jump)
ctrl-o / ctrl-i   navigate jump list back/forward
```

## Folds

```
za    toggle fold at cursor
zM    close all folds
zR    open all folds
zo    open fold at cursor
zc    close fold at cursor
```

## Marks

```
ma    set mark 'a' at cursor
'a    jump to line of mark 'a'
`a    jump to exact position of mark 'a'
:marks  list all marks
```

## Registers

```
"ayy   yank current line into register a
"ap    paste from register a
"+     system clipboard register
"0     always contains the last yanked text (not deleted)
```

`"0p` is extremely useful when you've yanked text and then done several deletes — the yank register stays clean.
