# Redis data structures — practical notes

Not a comprehensive guide, just the structures I use and what they're actually good for.

## Strings

```redis
SET key value [EX seconds] [NX|XX]
GET key
INCR counter    -- atomic increment
APPEND key val
STRLEN key
```

Good for: counters, cached values, feature flags, distributed locks (with EX and NX).

## Lists (doubly-linked)

```redis
LPUSH list val [val ...]  -- prepend
RPUSH list val [val ...]  -- append
LPOP / RPOP               -- remove from ends
LRANGE list 0 -1          -- all elements
LLEN list
BRPOP list timeout        -- blocking pop (worker queue pattern)
```

Good for: queues (RPUSH + BLPOP), message log, activity feeds with capped size (LTRIM).

## Sets

```redis
SADD set member [member ...]
SMEMBERS set
SISMEMBER set member
SCARD set
SUNION s1 s2
SINTER s1 s2
SDIFF s1 s2
```

Good for: unique visitors, tags, unique items, set operations without fetching to application layer.

## Sorted sets

```redis
ZADD zset score member
ZRANGE zset 0 -1 WITHSCORES
ZRANK zset member          -- 0-based rank
ZINCRBY zset incr member
ZRANGEBYSCORE zset min max
```

Good for: leaderboards, rate limiting by time window, priority queues.

## Hashes

```redis
HSET hash field value [field value ...]
HGET hash field
HGETALL hash
HMGET hash f1 f2
HINCRBY hash field amount
```

Good for: object storage where you need partial reads, counters per-entity.

## Expiry on any type

```redis
EXPIRE key seconds
TTL key        -- remaining time, -1 if no expiry, -2 if missing
PERSIST key    -- remove expiry
```

Docs: https://redis.io/docs/data-types/
