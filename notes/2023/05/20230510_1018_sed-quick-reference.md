---
title: Sed quick reference
slug: sed-quick-reference
tags: [cli, unix, reference]
description: Common sed patterns for in-place editing and stream transformation
---

# Sed quick reference

`sed` operates on a stream line by line. Each line is loaded into the pattern space, commands run, and the result is printed (unless `-n` suppresses output).

## Substitution

```
sed 's/old/new/'          # first occurrence per line
sed 's/old/new/g'         # all occurrences
sed 's/old/new/2'         # second occurrence only
sed 's/old/new/I'         # case-insensitive (GNU sed)
```

## In-place editing

GNU sed: `sed -i 's/old/new/' file.txt`  
macOS sed: `sed -i '' 's/old/new/' file.txt` — the empty string is required.

Always back up before in-place edits on production files. A common pattern is `-i.bak` which writes a backup alongside the original.

## Address ranges

```
sed '5,10s/old/new/'      # lines 5 through 10
sed '/start/,/end/d'      # delete between patterns (inclusive)
sed '/pattern/!d'         # delete lines NOT matching pattern
```

## Print and delete

```
sed -n '5p'               # print only line 5
sed '3d'                  # delete line 3
sed '/^$/d'               # delete blank lines
```

## Hold space

The hold space is a secondary buffer. `h` copies pattern to hold, `H` appends. `g` copies hold to pattern, `G` appends. Useful for reversing line order or joining lines, though `awk` is usually clearer for multi-line work.

## Gotchas

- The delimiter `/` can be replaced with any character: `s|/usr/local|/opt|g` avoids escaping.
- BSD and GNU sed differ subtly; scripts meant to run on both need testing on macOS.

Official docs: [GNU sed manual](https://www.gnu.org/software/sed/manual/sed.html).
