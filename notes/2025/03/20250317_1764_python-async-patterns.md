# Python async/await patterns

Working reference for async patterns I keep needing in the API work.

## Basic coroutine

```python
import asyncio

async def fetch_data(url: str) -> dict:
    # async work here
    await asyncio.sleep(1)  # simulate IO
    return {"url": url}

# Run a coroutine
result = asyncio.run(fetch_data("https://example.com"))
```

## Concurrent requests

```python
import asyncio
import httpx

async def fetch_all(urls: list[str]) -> list[dict]:
    async with httpx.AsyncClient() as client:
        tasks = [client.get(url) for url in urls]
        responses = await asyncio.gather(*tasks)
        return [r.json() for r in responses]
```

## Error handling with gather

```python
# return_exceptions=True prevents one failure from cancelling others
results = await asyncio.gather(*tasks, return_exceptions=True)
for r in results:
    if isinstance(r, Exception):
        print(f"Task failed: {r}")
    else:
        process(r)
```

## Semaphore for concurrency control

```python
sem = asyncio.Semaphore(10)  # max 10 concurrent

async def limited_fetch(client, url):
    async with sem:
        return await client.get(url)
```

## Timeout

```python
try:
    result = await asyncio.wait_for(slow_coroutine(), timeout=5.0)
except asyncio.TimeoutError:
    print("Request timed out")
```

## TaskGroup (Python 3.11+)

```python
async with asyncio.TaskGroup() as tg:
    task1 = tg.create_task(fetch("url1"))
    task2 = tg.create_task(fetch("url2"))
# all tasks complete (or first exception propagates)
result1 = task1.result()
```

Official docs: [https://docs.python.org/3/library/asyncio.html](https://docs.python.org/3/library/asyncio.html)
