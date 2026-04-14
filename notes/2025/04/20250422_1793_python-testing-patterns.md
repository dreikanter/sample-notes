# Python testing patterns — pytest reference

Patterns I use regularly in pytest. Covers the things that aren't obvious from the docs.

## Fixtures

```python
import pytest

@pytest.fixture
def db_connection():
    conn = create_connection()
    yield conn           # test runs here
    conn.close()         # teardown after yield

# Scoping: function (default), class, module, session
@pytest.fixture(scope="session")
def app_client():
    return TestClient(app)
```

## Parametrize

```python
@pytest.mark.parametrize("email,is_valid", [
    ("user@example.com", True),
    ("not-an-email", False),
    ("@missing-local.com", False),
    ("missing-domain@", False),
])
def test_email_validation(email, is_valid):
    assert validate_email(email) == is_valid
```

## Mocking

```python
from unittest.mock import patch, MagicMock

def test_sends_email(mock_send):
    with patch("mymodule.send_email") as mock_send:
        result = process_signup(user)
        mock_send.assert_called_once_with(user.email, subject="Welcome")

# Or as a decorator
@patch("mymodule.requests.get")
def test_api_call(mock_get):
    mock_get.return_value = MagicMock(status_code=200, json=lambda: {"key": "value"})
    result = fetch_data()
    assert result["key"] == "value"
```

## Async tests

```python
import pytest
import pytest_asyncio

@pytest.mark.asyncio
async def test_async_operation():
    result = await async_function()
    assert result is not None
```

## Markers

```python
@pytest.mark.slow
@pytest.mark.integration
def test_database_integration():
    ...

# pytest.ini or pyproject.toml
[tool.pytest.ini_options]
markers = ["slow: slow tests", "integration: integration tests"]

# Run specific markers
# pytest -m "not slow"
```

## conftest.py

Share fixtures across test files without importing. pytest auto-discovers `conftest.py` at each directory level.

Official docs: [https://docs.pytest.org](https://docs.pytest.org)
