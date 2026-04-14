---
title: Regex reference — patterns I use
slug: regex-reference
tags: [regex, reference, programming]
description: Regular expression patterns and notes for everyday use
---

# Regex reference — patterns I use

The patterns I write often enough to want a reference but not often enough to keep in memory.

## Character classes

```
\d    digit [0-9]
\w    word character [a-zA-Z0-9_]
\s    whitespace (space, tab, newline)
\D    non-digit
\W    non-word character
\S    non-whitespace
.     any character except newline
```

## Anchors and boundaries

```
^     start of string (or start of line in multiline mode)
$     end of string (or end of line in multiline mode)
\b    word boundary
\A    start of string (not affected by multiline)
\Z    end of string (not affected by multiline)
```

## Quantifiers

```
*     zero or more (greedy)
+     one or more (greedy)
?     zero or one
{3}   exactly 3
{3,}  3 or more
{3,6} 3 to 6
*?    zero or more (lazy / non-greedy)
+?    one or more (lazy)
```

## Groups and capturing

```
(abc)       capturing group
(?:abc)     non-capturing group
(?P<name>abc)  named capturing group (Python syntax)
(?=abc)     positive lookahead: matches if followed by abc
(?!abc)     negative lookahead: matches if NOT followed by abc
(?<=abc)    positive lookbehind: matches if preceded by abc
```

## Patterns I use often

```regex
# Email (simplified — perfect email regex is impossibly complex)
^[\w.+-]+@[\w-]+\.[\w.]+$

# ISO date
\d{4}-(?:0[1-9]|1[0-2])-(?:0[1-9]|[12]\d|3[01])

# UUID v4
[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}

# UK postcode
[A-Z]{1,2}[0-9][0-9A-Z]?\s?[0-9][A-Z]{2}
```

[Regex101](https://regex101.com/) for testing with explanation.
