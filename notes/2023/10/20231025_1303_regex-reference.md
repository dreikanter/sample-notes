# Regular expressions — reference for the patterns I keep looking up

I use https://regex101.com/ for testing. It explains each part of the pattern, which is invaluable for debugging.

## Character classes

```
\d    digit [0-9]
\w    word character [a-zA-Z0-9_]
\s    whitespace [\t\n\r\f\v ]
\D    non-digit
\W    non-word character
\S    non-whitespace
.     any character except newline (or any with DOTALL flag)
```

## Anchors

```
^     start of string (or line in MULTILINE mode)
$     end of string (or line in MULTILINE mode)
\b    word boundary
\B    non-word boundary
```

## Quantifiers

```
*     0 or more (greedy)
+     1 or more (greedy)
?     0 or 1 (greedy)
{n}   exactly n
{n,m} between n and m
*?    0 or more (lazy/non-greedy)
+?    1 or more (lazy)
```

**Greedy vs. lazy matters with nested patterns.** `<.+>` matches the whole `<a>foo</a>`. `<.+?>` matches just `<a>`.

## Groups

```
(abc)    capturing group
(?:abc)  non-capturing group (use this when you don't need the match)
(?P<name>abc)   named group (Python syntax)
(?<name>abc)    named group (JS/PCRE syntax)
```

## Lookahead and lookbehind

```
(?=foo)   positive lookahead: matches if followed by 'foo'
(?!foo)   negative lookahead: matches if NOT followed by 'foo'
(?<=foo)  positive lookbehind: matches if preceded by 'foo'
(?<!foo)  negative lookbehind: matches if NOT preceded by 'foo'
```

## Patterns I use often

```
Email (simplified): [\w.+-]+@[\w-]+\.[\w.]+
URL: https?://[^\s"'<>]+
ISO date: \d{4}-\d{2}-\d{2}
Hex color: #[0-9a-fA-F]{3,6}
IP address: \b\d{1,3}(\.\d{1,3}){3}\b
```
