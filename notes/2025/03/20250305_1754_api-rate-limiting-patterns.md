# API rate limiting — patterns and implementation notes

Writing this after implementing token bucket rate limiting for the v2 API. Notes on the decision process.

## Common algorithms

**Fixed window:** Count requests per time window (e.g., 100 per minute). Simple but has burst problems at window boundaries — a client can make 100 requests at 11:59 and 100 more at 12:00.

**Sliding window log:** Track exact timestamps of each request. Accurate but memory-intensive at scale (storing all timestamps per client).

**Sliding window counter:** Hybrid — use two fixed windows and weight them. More accurate than fixed window, less memory than log. Good practical choice.

**Token bucket:** Each client has a bucket of tokens. Requests consume tokens; tokens refill at a fixed rate. Allows bursting up to the bucket size. This is what we went with.

**Leaky bucket:** Requests enter a queue (bucket) and are processed at a fixed rate. Smooths traffic. Less common for APIs, more common for network QoS.

## Redis implementation of token bucket

```python
import redis
import time

def check_rate_limit(client_id: str, max_tokens: int, refill_rate: float) -> bool:
    r = redis.Redis()
    key = f"ratelimit:{client_id}"
    now = time.time()
    
    pipe = r.pipeline()
    pipe.hgetall(key)
    tokens_data = pipe.execute()[0]
    
    last_refill = float(tokens_data.get(b'last_refill', now))
    tokens = float(tokens_data.get(b'tokens', max_tokens))
    
    elapsed = now - last_refill
    tokens = min(max_tokens, tokens + elapsed * refill_rate)
    
    if tokens >= 1:
        tokens -= 1
        r.hset(key, mapping={'tokens': tokens, 'last_refill': now})
        r.expire(key, 3600)
        return True
    return False
```

## Response headers

Always communicate rate limit status to clients:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 43
X-RateLimit-Reset: 1709900400
Retry-After: 30  (on 429 responses)
```

Reference: [https://datatracker.ietf.org/doc/html/draft-ietf-httpapi-ratelimit-headers](https://datatracker.ietf.org/doc/html/draft-ietf-httpapi-ratelimit-headers)
