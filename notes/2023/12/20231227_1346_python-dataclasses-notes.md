# Python dataclasses practical notes

Been using dataclasses more intentionally since the team standardized on Python 3.11. Notes on the cases that surprised me.

## Basic pattern

```python
from dataclasses import dataclass, field

@dataclass
class Config:
    host: str
    port: int = 8080
    tags: list[str] = field(default_factory=list)
    debug: bool = False
```

The `field(default_factory=list)` is required for mutable defaults. Using `tags: list = []` raises `ValueError: mutable default ... is not allowed`. The error message is good but the gotcha is still common.

## Post-init

```python
@dataclass
class BoundedValue:
    value: float
    min_val: float
    max_val: float

    def __post_init__(self):
        if not self.min_val <= self.value <= self.max_val:
            raise ValueError(f"Value {self.value} out of range [{self.min_val}, {self.max_val}]")
```

`__post_init__` runs after the generated `__init__`. Clean place for validation.

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

Makes instances hashable and immutable. Useful for dictionary keys or set members. Attempting to set an attribute after creation raises `FrozenInstanceError`.

## Comparison with NamedTuple

Dataclasses are mutable by default, support inheritance, and allow methods naturally. NamedTuples are immutable, unpack as tuples, and are slightly faster for read-heavy cases. I use dataclasses for most things, NamedTuple when I specifically need tuple behavior.

Reference: [Python docs on dataclasses](https://docs.python.org/3/library/dataclasses.html).
