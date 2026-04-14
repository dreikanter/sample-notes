# Python dataclasses quick reference

Dataclasses were added in Python 3.7 (PEP 557) and significantly reduce boilerplate for classes that primarily hold data.

## Basic usage

```python
from dataclasses import dataclass, field

@dataclass
class Product:
    name: str
    price: float
    tags: list[str] = field(default_factory=list)
    in_stock: bool = True
```

The `@dataclass` decorator auto-generates `__init__`, `__repr__`, and `__eq__`.

## Field options

- `default`: scalar default value
- `default_factory`: callable that returns a new default (required for mutable defaults)
- `repr`: include in `__repr__` (default True)
- `compare`: include in `__eq__` and ordering (default True)
- `init`: include in `__init__` (default True)

## Frozen dataclasses

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float
```

Frozen instances are immutable and hashable.

## Post-init processing

```python
@dataclass
class Circle:
    radius: float
    area: float = field(init=False)

    def __post_init__(self):
        self.area = 3.14159 * self.radius ** 2
```

## Inheritance

Subclasses inherit parent fields. Note: fields with defaults in the parent cause issues if the subclass adds fields without defaults — Python will raise a TypeError.

## When to use dataclasses vs NamedTuple vs Pydantic

- `dataclass`: mutable, no validation, fast
- `NamedTuple`: immutable, tuple-like
- `pydantic.BaseModel`: validation, serialization, better for API schemas

PEP 557 reference: [https://peps.python.org/pep-0557/](https://peps.python.org/pep-0557/)
