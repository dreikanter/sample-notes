---
title: Vim motions reference
slug: vim-motions-reference
tags: [vim, editor, reference]
description: Vim motions I keep forgetting and looking up
---

# Vim motions reference

The motions I reach for and occasionally forget. Not the basics — just the ones I needed to look up.

## Text object motions

```
ci"    change inside quotes (works for any delimiter: ' ` ( [ { )
ca"    change around quotes (includes the quotes themselves)
vi{    visually select inside braces
da(    delete around parentheses
```

These compose with any operator: `d`, `c`, `y`, `v`.

## Jump list navigation

```
Ctrl-o    go to older position in jump list
Ctrl-i    go to newer position in jump list
g;        go to older position in change list
g,        go to newer position in change list
```

## Marks

```
ma        set mark 'a' at current position
`a        jump to mark 'a' (exact position)
'a        jump to line of mark 'a'
``        jump to last jump position
`.        jump to last edit position
```

## Window and buffer navigation

```
Ctrl-w h/j/k/l    move between splits
Ctrl-w =          equalize split sizes
:b <name>         jump to buffer matching name (with tab completion)
:ls               list open buffers
```

## Macros quick reference

```
qq    start recording macro to register q
q     stop recording
@q    play macro from register q
@@    replay last macro
5@q   play macro 5 times
```

[Practical Vim by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/) remains the best resource.
