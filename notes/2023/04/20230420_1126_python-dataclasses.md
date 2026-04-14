# Python dataclasses — practical notes

`dataclasses` (Python 3.7+) replace a lot of boilerplate `__init__`, `__repr__`, and `__eq__` code. Notes from converting a medium-sized codebase.

**Basic usage**

```python
from dataclasses import dataclass, field

@dataclass
class Config:
    host: str
    port: int = 8080
    tags: list[str] = field(default_factory=list)
```

**Field with default_factory** — always use `field(default_factory=...)` for mutable defaults. Never `tags: list = []` — that's shared across instances.

**Frozen dataclasses** (immutable, hashable)

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

**Post-init validation**

```python
@dataclass
class PositiveInt:
    value: int
    
    def __post_init__(self):
        if self.value <= 0:
            raise ValueError(f"Expected positive, got {self.value}")
```

**`asdict` and `astuple`**

```python
from dataclasses import asdict
d = asdict(config)  # Recursively converts to dict
```

**When to prefer over NamedTuple**

NamedTuple is lighter for read-only value objects. Dataclass wins when you need inheritance, post-init logic, or mutable state.

Full reference: [docs.python.org/3/library/dataclasses.html](https://docs.python.org/3/library/dataclasses.html)
