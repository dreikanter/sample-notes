# Regex reference

A working reference for regular expressions. I look up the same things repeatedly.

## Character classes

```
.      — any character except newline
\d     — digit [0-9]
\D     — non-digit
\w     — word character [a-zA-Z0-9_]
\W     — non-word character
\s     — whitespace (space, tab, newline, etc.)
\S     — non-whitespace
[abc]  — any of a, b, c
[^abc] — not a, b, or c
[a-z]  — range
```

## Anchors

```
^   — start of string (or line in multiline mode)
$   — end of string (or line in multiline mode)
\b  — word boundary
\B  — non-word boundary
```

## Quantifiers

```
*     — 0 or more (greedy)
+     — 1 or more (greedy)
?     — 0 or 1
{n}   — exactly n
{n,}  — n or more
{n,m} — between n and m
*?    — 0 or more (lazy)
+?    — 1 or more (lazy)
```

Greedy: matches as much as possible. Lazy: matches as little as possible.

## Groups and lookaround

```
(abc)        — capturing group
(?:abc)      — non-capturing group
(?=abc)      — positive lookahead
(?!abc)      — negative lookahead
(?<=abc)     — positive lookbehind
(?<!abc)     — negative lookbehind
```

## Practical patterns

```
Email (rough):     [^\s@]+@[^\s@]+\.[^\s@]+
URL (rough):       https?://[^\s]+
ISO date:          \d{4}-\d{2}-\d{2}
UUID:              [0-9a-f]{8}-[0-9a-f]{4}-...(etc)
US phone:          \+?1?\s*\(?\d{3}\)?[\s.-]?\d{3}[\s.-]?\d{4}
```

Interactive tester: https://regex101.com
