---
title: Python testing patterns — pytest notes
slug: python-testing-patterns
tags: [python, testing, reference]
public: true
---

# Python testing patterns — pytest notes

Notes from improving our test suite. pytest over unittest — simpler assertions, better fixtures, better output.

**Fixtures**

```python
import pytest

@pytest.fixture
def db_session():
    session = create_test_session()
    yield session
    session.rollback()
    session.close()

def test_user_created(db_session):
    user = create_user(db_session, name="Alice")
    assert user.id is not None
```

Fixtures with `yield` handle teardown cleanly.

**Parameterized tests**

```python
@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
    ("", ""),
])
def test_uppercase(input, expected):
    assert input.upper() == expected
```

**Mocking**

```python
from unittest.mock import patch, MagicMock

def test_sends_email(mock_email_client):
    with patch("myapp.email.client") as mock:
        send_welcome_email(user_id=1)
        mock.send.assert_called_once_with(
            to="user@example.com",
            subject="Welcome"
        )
```

Use `pytest-mock` for cleaner mock access via the `mocker` fixture.

**Testing exceptions**

```python
def test_invalid_input_raises():
    with pytest.raises(ValueError, match="must be positive"):
        process(-1)
```

**Markers and grouping**

```python
@pytest.mark.slow
def test_heavy_operation(): ...

# Run: pytest -m "not slow"
```

**`conftest.py`** — shared fixtures and hooks go here. Pytest discovers them automatically per directory.

[pytest documentation](https://docs.pytest.org/en/stable/)
