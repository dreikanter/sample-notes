# Regex practical reference

## Character classes

```
\d  — digit [0-9]
\w  — word character [a-zA-Z0-9_]
\s  — whitespace [ \t\n\r\f\v]
\D  — non-digit
\W  — non-word
\S  — non-whitespace
```

## Anchors and boundaries

```
^   — start of string (or line in multiline mode)
$   — end of string (or line in multiline mode)
\b  — word boundary
\A  — start of string (Python, not JS)
\Z  — end of string (Python, not JS)
```

## Quantifiers

```
*     — 0 or more (greedy)
+     — 1 or more (greedy)
?     — 0 or 1
{3}   — exactly 3
{2,5} — 2 to 5
*?    — 0 or more (lazy/non-greedy)
+?    — 1 or more (lazy)
```

## Groups and lookarounds

```
(abc)     — capturing group
(?:abc)   — non-capturing group
(?P<name>abc) — named group (Python)
(?<name>abc)  — named group (JS/PCRE)

(?=abc)   — positive lookahead: followed by abc
(?!abc)   — negative lookahead
(?<=abc)  — positive lookbehind: preceded by abc
(?<!abc)  — negative lookbehind
```

## Practical patterns

```
Email (simplified): [\w.+-]+@[\w-]+\.[a-zA-Z]{2,}
URL: https?://[\w./%-]+
ISO date: \d{4}-\d{2}-\d{2}
Time: ([01]\d|2[0-3]):[0-5]\d(:[0-5]\d)?
IPv4: (?:\d{1,3}\.){3}\d{1,3}
Hex color: #[0-9a-fA-F]{3,6}
```

## Gotchas

- `.` doesn't match newlines by default (use `re.DOTALL` in Python or `/s` flag)
- `^` and `$` match string start/end by default, not line start/end (use `re.MULTILINE`)
- Greedy by default: `<.+>` matches entire `<a>text</b>`, not just `<a>`

Tester: https://regex101.com/
