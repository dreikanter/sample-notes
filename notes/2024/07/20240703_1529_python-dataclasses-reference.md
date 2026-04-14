---
title: Python dataclasses reference
slug: python-dataclasses-reference
tags: [python, reference, programming]
description: Practical notes on Python dataclasses for everyday use
---

# Python dataclasses reference

Working notes on dataclasses after using them heavily in a refactor this week.

## Basic definition

```python
from dataclasses import dataclass, field

@dataclass
class Product:
    name: str
    price: float
    tags: list[str] = field(default_factory=list)
    in_stock: bool = True
```

Critical: mutable defaults must use `field(default_factory=...)` — a common gotcha.

## Post-init processing

```python
@dataclass
class Temperature:
    celsius: float
    fahrenheit: float = field(init=False)

    def __post_init__(self):
        self.fahrenheit = self.celsius * 9/5 + 32
```

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

Frozen instances are hashable and can be used as dict keys. Attempting to modify raises `FrozenInstanceError`.

## ClassVar for class-level attributes

```python
from typing import ClassVar
from dataclasses import dataclass

@dataclass
class Config:
    version: ClassVar[str] = "1.0"
    debug: bool = False
```

`ClassVar` fields are excluded from `__init__`, `__repr__`, etc.

## Comparison with NamedTuple

Dataclasses allow mutation (unless frozen), inheritance, and method definitions more naturally. NamedTuple is slightly faster to construct and is inherently iterable/unpackable. Use dataclasses as the default; reach for NamedTuple when you need tuple behavior or maximum performance.

Official docs: [dataclasses module](https://docs.python.org/3/library/dataclasses.html)
