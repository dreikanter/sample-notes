---
title: Python dataclasses — practical notes
slug: python-dataclasses
tags: [python, programming, reference]
description: Field options, post-init patterns, and gotchas with Python dataclasses.
---

# Python dataclasses — practical notes

Python's `dataclasses` module (3.7+) generates boilerplate for classes that primarily hold data. https://docs.python.org/3/library/dataclasses.html

## Basic usage

```python
from dataclasses import dataclass, field

@dataclass
class Book:
    title: str
    author: str
    year: int
    tags: list[str] = field(default_factory=list)
```

Never use a mutable default directly — use `field(default_factory=...)`.

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

Frozen instances are hashable and can be used as dict keys or in sets. Attempting mutation raises `FrozenInstanceError`.

## Post-init processing

```python
@dataclass
class Circle:
    radius: float
    area: float = field(init=False)

    def __post_init__(self):
        self.area = 3.14159 * self.radius ** 2
```

`field(init=False)` excludes a field from the constructor signature while still including it in `__repr__` and comparisons.

## Inheritance

Subclasses inherit parent fields. Fields with defaults must come after fields without. This constraint can cause pain when a parent class has non-defaulted fields and the child wants to add them — consider composition instead.

## Comparison with attrs and pydantic

- `attrs` offers more flexibility and has been around longer
- `pydantic` adds runtime validation and is ideal for config/API models
- plain `dataclasses` is best when you want zero dependencies and simple value objects

I've been reaching for dataclasses more since moving a project off pydantic — the startup time difference is measurable.
