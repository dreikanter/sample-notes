# Python async patterns

## Basic structure

```python
import asyncio

async def fetch_data(url: str) -> dict:
    await asyncio.sleep(1)  # simulate IO
    return {"url": url, "data": "..."}

asyncio.run(fetch_data("http://example.com"))
```

## Running multiple coroutines

```python
# Run concurrently, gather results
results = await asyncio.gather(
    fetch_data("http://a.com"),
    fetch_data("http://b.com"),
    fetch_data("http://c.com"),
)

# Same but continue on individual failures
results = await asyncio.gather(*coros, return_exceptions=True)
```

## Timeout

```python
try:
    result = await asyncio.wait_for(fetch_data(url), timeout=5.0)
except asyncio.TimeoutError:
    print("Request timed out")
```

## Semaphore for rate limiting

```python
sem = asyncio.Semaphore(10)  # max 10 concurrent

async def limited_fetch(url):
    async with sem:
        return await fetch_data(url)

# All 100 run, but at most 10 at once
results = await asyncio.gather(*[limited_fetch(url) for url in urls])
```

## Queue for producer-consumer

```python
async def producer(queue: asyncio.Queue):
    for i in range(10):
        await queue.put(i)
    await queue.put(None)  # sentinel

async def consumer(queue: asyncio.Queue):
    while True:
        item = await queue.get()
        if item is None:
            break
        print(f"Processing {item}")
        queue.task_done()

queue = asyncio.Queue()
await asyncio.gather(producer(queue), consumer(queue))
```

## Common mistake: blocking in async

```python
# Wrong: blocks the event loop
async def bad():
    time.sleep(5)  # BLOCKS
    result = requests.get(url)  # BLOCKS

# Right: use async equivalents
async def good():
    await asyncio.sleep(5)
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()
```

## asyncio.TaskGroup (Python 3.11+)

```python
async with asyncio.TaskGroup() as tg:
    task1 = tg.create_task(fetch_data("http://a.com"))
    task2 = tg.create_task(fetch_data("http://b.com"))
# Both done here, exceptions propagated
```

Docs: https://docs.python.org/3/library/asyncio.html
