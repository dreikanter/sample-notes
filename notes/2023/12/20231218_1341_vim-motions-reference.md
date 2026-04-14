# Vim motions reference

I use vim bindings everywhere but still reach for the mouse in specific situations where I don't know the motion. This is the living fix for that.

## Text objects

```
ciw   change inner word
caw   change a word (including whitespace)
ci"   change inside quotes
ca(   change a parenthetical (including parens)
dit   delete inner tag
yip   yank inner paragraph
```

Text objects are the thing that separates vim users who are actually fast from those who only think they are. `ciw` instead of `bce` is the kind of thing that compounds over thousands of edits.

## Navigation I underuse

```
H / M / L     top / middle / bottom of screen
zt / zz / zb  scroll to put cursor at top / center / bottom
gj / gk       move by visual line (for wrapped text)
%             jump to matching bracket/paren/brace
[[ / ]]       jump to previous/next function/section
```

## Macros

```
qa        start recording into register a
q         stop recording
@a        play back register a
10@a      play back 10 times
@@        repeat last macro
```

Macros for repetitive transformations are faster than sed for tasks that require any context about the surrounding code.

## Marks

```
ma    set mark a at current position
`a    jump to mark a (exact position)
'a    jump to line of mark a
``    jump to previous position (before last jump)
```

Reference: [Practical Vim by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/) is the book that filled in all the gaps, second edition.
