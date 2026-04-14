# Python asyncio — practical basics

Asyncio always confused me until I had to actually use it. Notes from converting a sync codebase to async.

**The mental model**

Async is cooperative multitasking on a single thread. A coroutine runs until it `await`s something, then yields control to the event loop, which can run other coroutines while the awaited thing completes. It's not parallelism — it's efficient waiting.

**Defining and running coroutines**

```python
import asyncio

async def fetch_data(url):
    # ... async operations ...
    return data

# Running from sync context:
asyncio.run(fetch_data("https://example.com"))
```

**Awaiting concurrently**

```python
# Sequential (slow):
a = await fetch(url_a)
b = await fetch(url_b)

# Concurrent (fast):
a, b = await asyncio.gather(fetch(url_a), fetch(url_b))
```

**Timeout**

```python
async with asyncio.timeout(5.0):
    result = await slow_operation()
```

(Python 3.11+; use `asyncio.wait_for` for earlier versions)

**Common pitfall: blocking calls**

```python
# BAD — blocks the entire event loop:
time.sleep(1)
result = requests.get(url)

# GOOD:
await asyncio.sleep(1)
async with httpx.AsyncClient() as client:
    result = await client.get(url)
```

Any synchronous I/O blocks all coroutines. Use `asyncio.to_thread()` to run blocking code without stopping the loop.

```python
result = await asyncio.to_thread(blocking_function, arg)
```

[Python asyncio docs](https://docs.python.org/3/library/asyncio.html)
