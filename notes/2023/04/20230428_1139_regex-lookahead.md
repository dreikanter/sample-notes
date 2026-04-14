---
title: Regex lookahead and lookbehind — reference
slug: regex-lookahead
tags: [regex, programming, reference]
---

# Regex lookahead and lookbehind — reference

Always need to look these up. Keeping a reference here.

**Lookahead**

```
foo(?=bar)    # Positive: foo followed by bar (bar not captured)
foo(?!bar)    # Negative: foo NOT followed by bar
```

Example: Match `100` only when followed by `px`:
```
\d+(?=px)
```
Against `"100px 200em"` → matches `100`.

**Lookbehind**

```
(?<=foo)bar    # Positive: bar preceded by foo (foo not captured)
(?<!foo)bar    # Negative: bar NOT preceded by foo
```

Example: Match price amounts preceded by `$`:
```
(?<=\$)\d+(\.\d{2})?
```
Against `"$29.99 and £15.00"` → matches `29.99`.

**Important caveats**

- Lookbehind in JavaScript requires ES2018+
- Python lookbehind must have a fixed width (no `*`, `+`, `?`)
- Safari added lookbehind support in Safari 16.4 (2023) — not available in older versions

**Non-capturing groups** (related but different):

```
(?:foo)bar    # Groups foo but doesn't capture it
```

The key distinction: lookaheads/lookbehinds assert position, they don't consume characters. Non-capturing groups still match the text, just don't create a capture group.

Full reference: [regular-expressions.info lookahead](https://www.regular-expressions.info/lookahead.html)
