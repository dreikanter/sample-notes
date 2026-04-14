# Python dataclasses — patterns I keep using

`@dataclass` was introduced in Python 3.7 and is now my default for simple data containers. Notes on the parts I actually use.

## Basic usage

```python
from dataclasses import dataclass, field
from typing import Optional

@dataclass
class Config:
    host: str
    port: int = 8080
    debug: bool = False
    tags: list[str] = field(default_factory=list)
```

`field(default_factory=...)` is required for mutable defaults—you can't put `[]` directly as a default.

## Post-init

```python
@dataclass
class Point:
    x: float
    y: float
    distance_from_origin: float = field(init=False)

    def __post_init__(self):
        self.distance_from_origin = (self.x**2 + self.y**2) ** 0.5
```

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Version:
    major: int
    minor: int
    patch: int

    def __str__(self):
        return f"{self.major}.{self.minor}.{self.patch}"
```

Frozen instances are hashable and can be used as dict keys.

## Inheritance gotcha

If a parent class has fields with defaults, all child class fields must also have defaults. Violating this raises `TypeError`. Common workaround: use `field(default=...)` throughout or switch to `@dataclass(kw_only=True)` (Python 3.10+).

## When not to use

- When you need complex validation: use Pydantic instead
- When the class has mostly methods and minimal data: plain class is fine
- When you need slots for memory efficiency: use `@dataclass(slots=True)` (3.10+)

Reference: [Python docs on dataclasses](https://docs.python.org/3/library/dataclasses.html)

