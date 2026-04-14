# Vim motions reference

Working reference. Focused on things I use daily and things I forget.

## Word motions

```
w    — next word start
b    — previous word start
e    — next word end
ge   — previous word end
W/B/E/gE  — same but for WORD (whitespace-delimited)
```

## Line motions

```
0    — start of line
^    — first non-blank character
$    — end of line
g_   — last non-blank character
```

## Find on line

```
f{c}  — forward to character c (inclusive)
F{c}  — backward to character c
t{c}  — forward to just before character c
T{c}  — backward to just after character c
;     — repeat last f/F/t/T
,     — reverse last f/F/t/T
```

These are underused. `dt"` (delete to next quote) is faster than counting words.

## Text objects (the good stuff)

```
iw/aw   — inner word / a word (includes surrounding space)
i"/a"   — inside/around quotes
i(/a(   — inside/around parens (also [ { < )
ip/ap   — inner/a paragraph
it/at   — inner/a tag (HTML/XML)
```

Combined with operators: `ci"` (change inside quotes), `da(` (delete around parens).

## Jumps

```
%    — jump to matching bracket/paren/brace
{/}  — previous/next blank line (paragraph)
]]   — next section
[[   — previous section
```

Reference: https://vim.rtorr.com/
Comprehensive guide: https://www.barbarianmeetscoding.com/boost-your-coding-fu-with-vscode-and-vim/cheatsheet/
