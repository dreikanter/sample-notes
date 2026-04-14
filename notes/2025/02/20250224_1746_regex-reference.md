# Regular expressions — working reference

Not comprehensive — just the patterns I blank on and have to look up.

## Character classes

```
.         any character except newline
\d        digit [0-9]
\D        non-digit
\w        word character [a-zA-Z0-9_]
\W        non-word character
\s        whitespace
\S        non-whitespace
```

## Anchors

```
^         start of string (or line in multiline mode)
$         end of string (or line in multiline mode)
\b        word boundary
\B        non-word boundary
```

## Quantifiers

```
*         0 or more (greedy)
+         1 or more (greedy)
?         0 or 1
{n}       exactly n
{n,m}     between n and m
*?        0 or more (lazy)
+?        1 or more (lazy)
```

## Groups

```
(...)     capturing group
(?:...)   non-capturing group
(?=...)   positive lookahead
(?!...)   negative lookahead
(?<=...)  positive lookbehind
(?<!...)  negative lookbehind
```

## Common patterns I actually use

```regex
# Email (permissive)
[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}

# ISO date
\d{4}-\d{2}-\d{2}

# URL
https?://[^\s]+

# Trailing whitespace
\s+$

# Consecutive duplicate words
\b(\w+)\s+\1\b
```

## Python regex flags

```python
import re
re.compile(pattern, re.IGNORECASE | re.MULTILINE | re.DOTALL)
```

Reference tool: [https://regex101.com](https://regex101.com) — essential for testing
