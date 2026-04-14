---
title: Vim motions reference card
slug: vim-motions-ref
tags: [vim, reference, tools]
---

# Vim motions reference card

The motions I still have to think about — the ones below the muscle memory threshold.

## Text objects

```
ciw   change inner word
caw   change a word (includes surrounding space)
ci"   change inside quotes
ca"   change a quotes (includes quotes)
cip   change inner paragraph
dib   delete inner block (inside parentheses)
yit   yank inner tag (HTML)
```

## Line navigation

```
0     beginning of line (before whitespace)
^     first non-blank character
$     end of line
g_    last non-blank character
```

## Window and screen

```
H     top of visible screen
M     middle of visible screen
L     bottom of visible screen
zt    scroll to put cursor at top
zz    scroll to put cursor at middle
zb    scroll to put cursor at bottom
Ctrl+u   scroll up half page
Ctrl+d   scroll down half page
```

## Marks

```
ma    set mark 'a' at current position
'a    jump to line of mark 'a'
`a    jump to exact position of mark 'a'
''    jump back to last jump position
``    jump back to last exact position
```

## Macros

```
qa    record macro into register 'a'
q     stop recording
@a    play macro 'a'
@@    replay last macro
10@a  play macro 'a' 10 times
```

## Registers

```
"ayy    yank line into register 'a'
"ap     paste from register 'a'
"+y     yank to system clipboard
"+p     paste from system clipboard
```

Interactive tutorial: [https://vimschool.netlify.app](https://vimschool.netlify.app)
