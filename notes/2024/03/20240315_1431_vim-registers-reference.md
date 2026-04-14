# Vim registers reference

Always forget the obscure ones. Keeping this here.

## Named registers

```
"a through "z     — named registers, manually set
"A through "Z     — append to named register (uppercase)

"ayy              — yank line into register a
"ap               — paste from register a
```

## Special registers

```
""    — unnamed (default) register: last yank or delete
"0    — yank register: last explicit yank (not affected by deletes)
"1-"9 — numbered: history of deletes, newest in "1
"-    — small delete register (< 1 line)
".    — last inserted text (read-only)
":    — last ex command (read-only)
"%    — current filename (read-only)
"#    — alternate filename (read-only)
"=    — expression register (prompts for eval)
"_    — black hole register (silent discard)
"*    — system clipboard (X11 selection)
"+    — system clipboard (clipboard proper)
"/    — last search pattern
```

## Practical patterns

```
"0p          — paste last yank even after subsequent deletes
"_d          — delete without affecting clipboard
:let @a = @+  — copy clipboard into register a
:reg          — inspect all registers
ctrl-r a      — in insert mode, paste register a
```

## The yank register problem

Classic vim frustration: you yank something, then delete something, now you paste and get the deleted thing instead of the yanked thing. Solution: always paste from `"0` if you need the yanked content after any deletions.

More detail: https://vim.fandom.com/wiki/Understanding_registers
