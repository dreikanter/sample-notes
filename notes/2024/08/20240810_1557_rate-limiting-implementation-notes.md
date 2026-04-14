# Rate limiting implementation — mid-build notes

Three weeks into actual implementation. Noting the decisions made and why.

## Token bucket vs sliding window

Went with sliding window log instead of token bucket, despite the spec originally specifying token bucket. Reason: token bucket allows bursting (up to the full bucket capacity at once), which is problematic for our export endpoints. Sliding window gives smoother, more predictable behavior.

The tradeoff: sliding window log requires storing a sorted set of timestamps per user in Redis, which is more memory than a simple counter. At scale this matters but we're not at scale yet.

## Redis data structure choice

```
ZADD user:{id}:requests {timestamp} {request_id}
ZREMRANGEBYSCORE user:{id}:requests 0 {window_start}
ZCARD user:{id}:requests
EXPIRE user:{id}:requests {window_seconds}
```

All four operations wrapped in a Lua script for atomicity. This was not optional — without atomic execution, race conditions at high concurrency would cause over-counting.

## Header format

Settled on the semi-standard:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1722902400
```

The `Reset` value is a Unix timestamp, not a relative seconds count. Several clients handle timestamps better.

## Still to do

- Metrics instrumentation — want to track p99 latency of the Redis calls
- Graceful handling when Redis is unavailable (fail open or fail closed?)

See todo list: [[20240705_1531]]

Reference: [IETF rate limiting headers draft](https://www.ietf.org/archive/id/draft-ietf-httpapi-ratelimit-headers-07.txt)
