# Vim motions reference

I keep having to look these up. Making a personal cheatsheet of the ones worth internalising. Reference: [Vim docs](https://vimdoc.sourceforge.net/htmldoc/motion.html).

## Movement

```
h j k l       left / down / up / right
w / b         next / previous word start
e             next word end
0 / ^         line start (col 0) / first non-whitespace
$             line end
gg / G        file start / file end
{num}G        go to line number
% 	          matching bracket
f{c} / F{c}   find char forward / backward on line
t{c} / T{c}   up-to char forward / backward
; / ,         repeat f/F/t/T forward / backward
```

## Editing operators (combine with motions)

```
d             delete
c             change (delete + enter insert)
y             yank (copy)
v             visual select
```

Operators + motions: `dw` (delete word), `c$` (change to end of line), `yy` (yank whole line), `dd` (delete line).

## Text objects

```
iw / aw       inner word / a word (with space)
i" / a"       inner / outer double-quoted string
i( / a(       inner / outer parentheses
ip / ap       inner / outer paragraph
it / at       inner / outer HTML tag
```

Examples: `ci"` changes inside quotes; `dat` deletes an HTML element including tags; `yip` yanks a paragraph.

## Useful patterns

```
.             repeat last change
*             search for word under cursor
n / N         next / previous search result
u / Ctrl+r    undo / redo
>>  /  <<     indent / dedent line
=             auto-indent (e.g., `=G` to indent file)
```

The [Practical Vim book by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/) changed how I think about editing more than anything else. The "dot formula" concept is the key insight.
