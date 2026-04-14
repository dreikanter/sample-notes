---
title: Python dataclasses reference
slug: python-dataclasses-ref
tags: [python, programming, reference]
description: Practical patterns for Python dataclasses
---

# Python dataclasses reference

## Basic usage

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Book:
    title: str
    author: str
    pages: int
    tags: List[str] = field(default_factory=list)
    read: bool = False
```

## Post-init processing

```python
@dataclass
class Temperature:
    celsius: float
    fahrenheit: float = field(init=False)

    def __post_init__(self):
        self.fahrenheit = self.celsius * 9/5 + 32
```

## Frozen (immutable) dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
    # Instances are hashable, can be used as dict keys
```

## Ordering

```python
@dataclass(order=True)
class Priority:
    level: int  # compared first
    label: str  # compared second if level equals
```

## Converting to dict/tuple

```python
from dataclasses import asdict, astuple

book = Book("Dune", "Herbert", 896)
d = asdict(book)   # {'title': 'Dune', 'author': 'Herbert', ...}
t = astuple(book)  # ('Dune', 'Herbert', 896, [], False)
```

## Gotcha: mutable defaults

Never do `tags: List[str] = []` — this will raise `ValueError`. Always use `field(default_factory=list)`. The error message is actually pretty clear, which is rare.

## When to use vs NamedTuple

Use `dataclass` when you need mutability or inheritance. Use `NamedTuple` when you want dict-style unpacking and tuple compatibility. Frozen dataclasses and NamedTuples are similar in practice.

Docs: https://docs.python.org/3/library/dataclasses.html
