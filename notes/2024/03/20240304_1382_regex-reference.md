# Regex reference — patterns I use and keep forgetting

Not a comprehensive guide. Just the patterns I reach for and have to look up.

## Character classes

```
\d    digit [0-9]
\w    word character [a-zA-Z0-9_]
\s    whitespace
\D    non-digit
\W    non-word
\S    non-whitespace
```

## Anchors

```
^     start of string (or line in multiline mode)
$     end of string (or line in multiline mode)
\b    word boundary
\B    non-word boundary
```

`\b` is the one that saves time on whole-word matching. Without it, `error` matches inside `errors`, `error_code`, etc.

## Quantifiers

```
*     zero or more (greedy)
+     one or more (greedy)
?     zero or one
{3}   exactly 3
{2,5} between 2 and 5
*?    zero or more (lazy/non-greedy)
+?    one or more (lazy/non-greedy)
```

The greedy vs lazy distinction trips up anyone who doesn't use regex daily. Greedy (`.*`) matches as much as possible; lazy (`.*?`) matches as little as possible.

## Groups and lookaheads

```
(abc)         capturing group
(?:abc)       non-capturing group
(?=abc)       positive lookahead — matches if followed by abc
(?!abc)       negative lookahead — matches if NOT followed by abc
(?<=abc)      positive lookbehind
(?<!abc)      negative lookbehind
```

## Patterns I keep reconstructing

```
Email (approximate): [\w.+-]+@[\w-]+\.[a-z]{2,}
ISO date: \d{4}-\d{2}-\d{2}
Slug: [a-z][a-z0-9-]*[a-z0-9]
UUID: [0-9a-f]{8}-(?:[0-9a-f]{4}-){3}[0-9a-f]{12}
```

Reference: [regex101.com](https://regex101.com/) for live testing with explanation; [rexegg.com](https://www.rexegg.com/) for advanced patterns.
