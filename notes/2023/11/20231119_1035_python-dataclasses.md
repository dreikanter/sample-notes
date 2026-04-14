# Python dataclasses — practical notes

Added in Python 3.7, dataclasses have largely replaced the pattern of writing `__init__`, `__repr__`, and `__eq__` by hand for simple data-holding classes. These are the patterns I actually use day to day.

## Basic usage

```python
from dataclasses import dataclass, field

@dataclass
class Config:
    host: str
    port: int = 8080
    tags: list[str] = field(default_factory=list)
```

`field(default_factory=list)` is required for mutable defaults — using `tags: list = []` directly raises a `ValueError`.

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

Frozen instances are hashable and can be used as dict keys or set members. Attempting to assign to a field raises `FrozenInstanceError`.

## Post-init processing

```python
@dataclass
class BoundedValue:
    value: float
    min_val: float = 0.0
    max_val: float = 1.0

    def __post_init__(self):
        if not (self.min_val <= self.value <= self.max_val):
            raise ValueError(f"{self.value} outside [{self.min_val}, {self.max_val}]")
```

## Conversion helpers

```python
from dataclasses import asdict, astuple

asdict(config)    # returns a dict (deep copy)
astuple(config)   # returns a tuple
```

## When not to use them

If you need validators, coercion, or serialisation built in, reach for `pydantic` or `attrs` instead. Dataclasses have no built-in validation beyond what you write in `__post_init__`.

See also the `__slots__` option (Python 3.10+) for memory efficiency with large numbers of instances: `@dataclass(slots=True)`.

Full reference: [https://docs.python.org/3/library/dataclasses.html](https://docs.python.org/3/library/dataclasses.html)
