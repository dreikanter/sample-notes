---
title: Python async/await basics
slug: python-async-basics
tags: [python, async, programming, reference]
description: Practical notes on asynchronous Python with asyncio.
---

# Python async/await basics

Working notes on Python's async model. The [official asyncio documentation](https://docs.python.org/3/library/asyncio.html) is good but dense; this is more practical.

## The core model

Python's async is cooperative multitasking. An `async def` function (a coroutine) can be paused at `await` points to let other coroutines run. This is not true parallelism (that's `multiprocessing`) — it's concurrency, good for I/O-bound tasks.

```python
import asyncio

async def fetch_data(url: str) -> dict:
    # At this await, other coroutines can run
    response = await some_http_client.get(url)
    return response.json()

async def main():
    # Run one at a time:
    data1 = await fetch_data("https://api.example.com/1")
    data2 = await fetch_data("https://api.example.com/2")

    # Run concurrently:
    data1, data2 = await asyncio.gather(
        fetch_data("https://api.example.com/1"),
        fetch_data("https://api.example.com/2"),
    )

asyncio.run(main())
```

## Key functions

```python
asyncio.run(coro)              # Entry point — runs a coroutine
asyncio.gather(*coros)         # Run multiple coroutines concurrently
asyncio.create_task(coro)      # Schedule without awaiting immediately
asyncio.sleep(seconds)         # Async sleep (don't use time.sleep!)
asyncio.wait_for(coro, timeout=5.0)  # Coroutine with timeout
```

## Common mistake: calling sync code that blocks

```python
async def bad():
    time.sleep(10)  # Blocks the entire event loop — wrong
    await asyncio.sleep(10)  # Correct — yields control

# For truly blocking code (file I/O, CPU work):
loop = asyncio.get_event_loop()
result = await loop.run_in_executor(None, blocking_function)
```

## When to use async

- HTTP requests (use `httpx` or `aiohttp`)
- Database queries (use `asyncpg` or `databases`)
- WebSocket servers
- Any I/O-bound service handling many concurrent connections

Not useful for CPU-bound work — use multiprocessing for that.
