# Python dataclasses reference

## Basic usage

```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float
    label: str = "unnamed"
```

Auto-generates `__init__`, `__repr__`, `__eq__`. That's most of what you need from a data container class.

## Field options

```python
@dataclass
class Config:
    host: str
    port: int = 8080
    tags: list = field(default_factory=list)  # mutable default
    _cache: dict = field(default_factory=dict, repr=False, compare=False)
```

Never use mutable objects as defaults directly — use `field(default_factory=...)`.

`repr=False`: exclude from `__repr__`.
`compare=False`: exclude from `__eq__` and ordering.
`init=False`: exclude from `__init__`, set in `__post_init__`.

## Post-init processing

```python
@dataclass
class Circle:
    radius: float
    area: float = field(init=False)

    def __post_init__(self):
        self.area = 3.14159 * self.radius ** 2
```

## Frozen (immutable)

```python
@dataclass(frozen=True)
class ImmutablePoint:
    x: float
    y: float
```

Frozen dataclasses are hashable (usable as dict keys / set members).

## Ordering

```python
@dataclass(order=True)
class Version:
    major: int
    minor: int
    patch: int
```

Generates `__lt__`, `__le__`, `__gt__`, `__ge__` based on field order.

## vs NamedTuple

Dataclasses are mutable by default, support inheritance, support methods naturally. NamedTuple is immutable, is an actual tuple (position-accessible, unpackable), and may have marginal performance advantages. Use dataclass unless you need tuple behavior.

Docs: https://docs.python.org/3/library/dataclasses.html
