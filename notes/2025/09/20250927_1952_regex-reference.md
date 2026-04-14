# Regex reference for everyday use

Not comprehensive—just the patterns I actually use and keep forgetting.

## Anchors and boundaries

```
^       start of string (or line in multiline mode)
$       end of string (or line)
\b      word boundary
\B      not a word boundary
```

## Character classes

```
\d      digit (0-9)
\D      not digit
\w      word character (a-z, A-Z, 0-9, _)
\W      not word character
\s      whitespace (space, tab, newline, etc.)
\S      not whitespace
[aeiou] any vowel
[^aeiou] any non-vowel
[a-z]   lowercase letter
```

## Quantifiers

```
*       zero or more (greedy)
+       one or more (greedy)
?       zero or one
{n}     exactly n
{n,}    n or more
{n,m}   between n and m
*?      zero or more (lazy — as few as possible)
```

## Groups and lookarounds

```
(abc)           capture group
(?:abc)         non-capturing group
(?=abc)         lookahead — position followed by abc
(?!abc)         negative lookahead
(?<=abc)        lookbehind — position preceded by abc
(?<!abc)        negative lookbehind
```

## Patterns I use constantly

```regex
# Email (rough but useful)
[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}

# ISO date
\d{4}-\d{2}-\d{2}

# URL
https?://[^\s"'<>]+

# Trailing whitespace
\s+$

# Blank lines
^\s*$
```

Reference: [regex101.com](https://regex101.com/) for interactive testing with explanations.

