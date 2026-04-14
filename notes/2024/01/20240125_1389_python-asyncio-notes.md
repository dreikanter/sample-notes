# Python asyncio — practical notes

The new service I'm prototyping uses async Python. These are things I've had to relearn after six months away from it.

## The fundamental model

```python
import asyncio

async def fetch(url: str) -> str:
    await asyncio.sleep(1)  # simulating I/O wait
    return f"result from {url}"

async def main():
    results = await asyncio.gather(
        fetch("https://example.com/a"),
        fetch("https://example.com/b"),
        fetch("https://example.com/c"),
    )
    print(results)  # all three run concurrently

asyncio.run(main())
```

`asyncio.gather` runs coroutines concurrently. Without it, `await fetch(url)` runs each one sequentially — the most common asyncio mistake.

## When asyncio helps and when it doesn't

Asyncio is for I/O-bound work: network calls, file reads, database queries. For CPU-bound work (computation, data processing), asyncio adds overhead without helping — use multiprocessing instead.

The canonical failure: using asyncio for a data transformation pipeline that does no I/O. Slower than synchronous code, harder to understand.

## Error handling in gather

```python
results = await asyncio.gather(
    fetch_a(),
    fetch_b(),
    return_exceptions=True  # failed coroutines return exceptions instead of raising
)
```

Without `return_exceptions=True`, one failure cancels all gathered tasks. Usually not what you want.

## asyncio.TaskGroup (Python 3.11+)

```python
async with asyncio.TaskGroup() as tg:
    task_a = tg.create_task(fetch_a())
    task_b = tg.create_task(fetch_b())

# both tasks are done here
print(task_a.result(), task_b.result())
```

Cleaner than gather for cases where you need individual task results.

Reference: [Python asyncio documentation](https://docs.python.org/3/library/asyncio.html) and [Lynn Root's asyncio tutorial](https://www.roguelynn.com/words/asyncio-we-did-it-wrong/) for practical patterns.
