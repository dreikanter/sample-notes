# API rate limiting patterns

Notes from implementing rate limiting on the service. Three main algorithm choices.

## Token bucket

A bucket holds up to N tokens. Refills at rate R tokens/second. Each request consumes one token. If empty, reject.

**Properties:** Allows bursting up to bucket capacity. Smooth refill rather than block resets. Good for APIs that can handle occasional bursts.

```python
import time

class TokenBucket:
    def __init__(self, capacity: int, refill_rate: float):
        self.capacity = capacity
        self.tokens = capacity
        self.refill_rate = refill_rate  # tokens per second
        self.last_refill = time.monotonic()

    def consume(self, tokens: int = 1) -> bool:
        now = time.monotonic()
        elapsed = now - self.last_refill
        self.tokens = min(self.capacity, self.tokens + elapsed * self.refill_rate)
        self.last_refill = now

        if self.tokens >= tokens:
            self.tokens -= tokens
            return True
        return False
```

## Sliding window log

Store timestamp of each request. On new request, remove timestamps older than window, count remaining. If under limit, allow.

**Properties:** Precise. Memory-expensive at high traffic (stores every request timestamp). Better for low-volume APIs with strict correctness requirements.

## Fixed window counter

Increment counter per window period. Reset at window boundary.

**Properties:** Simple, low memory. Allows up to 2x the rate limit at window boundaries (requests at end of period 1 + requests at start of period 2). The boundary problem makes this unsuitable for strict rate limiting.

## Redis implementation (sliding window)

```python
import redis
import time

def is_rate_limited(user_id: str, limit: int, window_seconds: int) -> bool:
    r = redis.Redis()
    now = time.time()
    key = f"rl:{user_id}"
    pipe = r.pipeline()
    pipe.zremrangebyscore(key, 0, now - window_seconds)
    pipe.zadd(key, {str(now): now})
    pipe.zcard(key)
    pipe.expire(key, window_seconds)
    results = pipe.execute()
    return results[2] > limit
```

Headers to return: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`, `Retry-After` (on 429).

Reference: https://www.figma.com/blog/an-alternative-approach-to-rate-limiting/
