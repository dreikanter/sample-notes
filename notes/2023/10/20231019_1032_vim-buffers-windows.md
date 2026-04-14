# Vim buffers, windows, and tabs

Notes after spending an afternoon actually learning how these three concepts relate, rather than muddling through as I have been.

## Core distinctions

A **buffer** is an in-memory copy of a file (or an unnamed scratch space). Opening a file always creates a buffer. Closing a window does not destroy the buffer — it just hides it.

A **window** is a viewport onto a buffer. You can have multiple windows showing the same buffer, or different buffers side by side.

A **tab page** is a collection of windows. Tabs in Vim are not like browser tabs (one file per tab) — they're more like named window layouts.

## Buffer navigation

```
:ls           " list all buffers
:b 3          " switch to buffer 3
:bn / :bp     " next / previous buffer
:bd           " delete (close) a buffer
:b#           " switch to alternate buffer
```

## Window splits

```
Ctrl-w s      " horizontal split
Ctrl-w v      " vertical split
Ctrl-w w      " cycle through windows
Ctrl-w h/j/k/l  " move between windows (vim direction keys)
Ctrl-w =      " equalize window sizes
Ctrl-w q      " close current window
```

## Tab pages

```
:tabnew       " open a new tab
gt / gT       " next / previous tab
:tabclose     " close current tab
:tabs         " list all tabs
```

## Practical workflow

For most work I now use splits rather than tabs — keeps related files visible simultaneously. Tabs are useful for completely separate contexts (e.g. working in two different directories).

The `:args` command combined with `:argdo` is worth knowing for batch operations across multiple files.

Full reference: [https://vimhelp.org/windows.txt.html](https://vimhelp.org/windows.txt.html)
