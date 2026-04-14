---
title: Python async/await — patterns I actually use
slug: python-async-patterns
tags: [python, async, programming]
description: Practical async patterns for Python 3.10+, focusing on what I use in real code.
public: true
---

# Python async/await — patterns I actually use

Reference: https://docs.python.org/3/library/asyncio.html

## Basic coroutine

```python
import asyncio

async def fetch_data(url: str) -> dict:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()
```

## Concurrent requests with gather

```python
async def fetch_all(urls: list[str]) -> list[dict]:
    tasks = [fetch_data(url) for url in urls]
    return await asyncio.gather(*tasks)
```

`gather()` runs coroutines concurrently. This is the main use case: I/O-bound work that would be slow sequentially.

## Error handling with gather

By default, `gather()` cancels all tasks if one raises. Use `return_exceptions=True` to get results even when some fail:

```python
results = await asyncio.gather(*tasks, return_exceptions=True)
for result in results:
    if isinstance(result, Exception):
        logger.error(f"Task failed: {result}")
    else:
        process(result)
```

## Timeouts

```python
async def fetch_with_timeout(url: str) -> dict:
    async with asyncio.timeout(10):  # Python 3.11+
        return await fetch_data(url)
```

Or for 3.10: `asyncio.wait_for(coro, timeout=10)`.

## Queues for producer-consumer patterns

```python
async def producer(queue: asyncio.Queue, items: list):
    for item in items:
        await queue.put(item)
    await queue.put(None)  # sentinel

async def consumer(queue: asyncio.Queue):
    while True:
        item = await queue.get()
        if item is None:
            break
        await process(item)
        queue.task_done()
```

## When NOT to use async

CPU-bound work. `asyncio` is single-threaded — it doesn't parallelize computation. Use `concurrent.futures.ProcessPoolExecutor` for CPU-bound tasks, or `run_in_executor` to run synchronous I/O in a thread pool without blocking the event loop.
