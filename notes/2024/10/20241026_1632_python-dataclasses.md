# Python dataclasses — practical notes

Dataclasses were added in Python 3.7 and have become my default for simple data-holding objects. Reference: [Python docs on dataclasses](https://docs.python.org/3/library/dataclasses.html).

## Basic usage

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Article:
    title: str
    author: str
    tags: List[str] = field(default_factory=list)
    published: bool = False
    word_count: int = 0
```

`field(default_factory=list)` is necessary for mutable defaults — using `tags: List[str] = []` directly raises a `ValueError`.

## Useful options

```python
@dataclass(frozen=True)  # Immutable — makes it hashable
@dataclass(order=True)   # Adds __lt__, __le__, __gt__, __ge__
@dataclass(slots=True)   # Python 3.10+, more memory-efficient
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

## When to prefer dataclasses over alternatives

- Over plain dicts: when you want attribute access, type hints, and IDE support
- Over NamedTuple: when you need mutability (or use `frozen=True` if you want immutability with some inheritance capabilities)
- Over Pydantic: when you don't need runtime validation and want zero dependencies

Pydantic is worth the dependency when you're parsing external data (JSON from an API, user input) and want automatic validation and coercion. For internal data structures, dataclasses are usually sufficient and lighter.

## `asdict` and `astuple`

```python
from dataclasses import asdict, astuple

article = Article(title="Draft", author="Alex")
print(asdict(article))  # {'title': 'Draft', 'author': 'Alex', ...}
```
