---
title: Redis patterns — reference for common use cases
slug: redis-patterns
tags: [redis, database, reference, caching]
description: Redis patterns I use regularly — caching, rate limiting, distributed locks, pub/sub.
public: true
---

# Redis patterns — reference for common use cases

Reference: https://redis.io/docs/

## Caching with TTL

```python
import redis
import json

r = redis.Redis(host='localhost', port=6379, decode_responses=True)

def get_user(user_id: int) -> dict:
    cache_key = f"user:{user_id}"
    cached = r.get(cache_key)
    if cached:
        return json.loads(cached)

    user = fetch_from_db(user_id)  # your DB call
    r.setex(cache_key, 3600, json.dumps(user))  # 1-hour TTL
    return user
```

## Rate limiting (token bucket approximation)

```python
def is_rate_limited(user_id: str, limit: int = 100, window: int = 3600) -> bool:
    key = f"ratelimit:{user_id}:{window}"
    count = r.incr(key)
    if count == 1:
        r.expire(key, window)
    return count > limit
```

Simple sliding window. For true token bucket, use a Lua script to atomically check and decrement.

## Distributed lock

```python
import uuid

def acquire_lock(lock_name: str, timeout: int = 10) -> str | None:
    lock_id = str(uuid.uuid4())
    acquired = r.set(f"lock:{lock_name}", lock_id, nx=True, ex=timeout)
    return lock_id if acquired else None

def release_lock(lock_name: str, lock_id: str) -> bool:
    # Lua for atomic check-and-delete
    script = """
    if redis.call("get", KEYS[1]) == ARGV[1] then
        return redis.call("del", KEYS[1])
    else
        return 0
    end
    """
    return bool(r.eval(script, 1, f"lock:{lock_name}", lock_id))
```

The Lua script is important — without it, you can delete another process's lock.

## Pub/Sub for lightweight messaging

```python
# Publisher
r.publish('events:user-created', json.dumps({'user_id': 123}))

# Subscriber
p = r.pubsub()
p.subscribe('events:user-created')
for message in p.listen():
    if message['type'] == 'message':
        handle_event(json.loads(message['data']))
```

Note: pub/sub doesn't persist messages. Use Redis Streams for durable messaging.
