---
title: Python asyncio — mental model and practical patterns
slug: python-async-notes
tags: [python, async, programming]
description: Getting the mental model right for Python's async
public: true
---

# Python asyncio — mental model and practical patterns

The mental model took me longer to build than it should have. These are the notes that would have helped me earlier.

**The key insight**

asyncio is cooperative multitasking, not parallelism. When you `await` something, you yield control to the event loop, which can run other coroutines. But only one coroutine runs at a time. For I/O-bound work (network calls, database queries), this is fine because most of the waiting is in the OS anyway. For CPU-bound work, asyncio doesn't help — use multiprocessing.

**Coroutines, tasks, and futures**

A coroutine is a function defined with `async def`. It doesn't run when you call it; it returns a coroutine object.

A task is a coroutine that has been scheduled on the event loop. Create with `asyncio.create_task()`.

```python
async def fetch(url):
    async with httpx.AsyncClient() as client:
        return await client.get(url)

# This runs sequentially — each await waits for completion
result1 = await fetch(url1)
result2 = await fetch(url2)

# This runs concurrently — both fetches start before either finishes
task1 = asyncio.create_task(fetch(url1))
task2 = asyncio.create_task(fetch(url2))
result1, result2 = await asyncio.gather(task1, task2)
```

**Common mistake: blocking the event loop**

Any synchronous blocking call (a slow CPU computation, `time.sleep()`) blocks the entire event loop. Use `asyncio.sleep()` for delays and `loop.run_in_executor()` for blocking I/O.

**Exception handling with gather**

By default, `gather` raises the first exception and cancels the others. Use `return_exceptions=True` to collect all exceptions.

Python asyncio docs: https://docs.python.org/3/library/asyncio.html
