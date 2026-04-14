---
title: Redis use cases and patterns
slug: redis-use-cases
tags: [redis, databases, caching, backend]
public: true
---

# Redis use cases and patterns

Notes from using Redis across several projects. See [Redis documentation](https://redis.io/docs/) for the full reference.

## When Redis is the right tool

**Caching**: the classic use case. Store expensive query results or API responses with a TTL. The LRU eviction policy means it handles memory pressure gracefully.

**Session storage**: fast, temporary, appropriate scale. Much better than storing sessions in a relational database.

**Rate limiting**: use Redis atomic operations (INCR + EXPIRE) to implement sliding window rate limits without race conditions.

**Pub/Sub**: lightweight message passing between services. Not a full message queue (no persistence guarantees), but sufficient for real-time notification patterns.

**Leaderboards**: sorted sets are purpose-built for ranked lists:
```
ZADD leaderboard 1500 "alice"
ZADD leaderboard 1200 "bob"
ZREVRANGE leaderboard 0 9 WITHSCORES  # Top 10
ZRANK leaderboard "alice"             # Rank of alice
```

**Distributed locks**: using SETNX with a TTL for mutex patterns across services:
```
SET lockname value NX PX 30000  # Set only if not exists, expire in 30s
```

## Key data structures

- **Strings**: bytes (JSON, serialised objects, simple counters)
- **Lists**: ordered, allow duplicates — job queues, activity feeds
- **Sets**: unique members — tag clouds, follower lists
- **Sorted sets**: unique members with a score — leaderboards, time-series with scores
- **Hashes**: field-value pairs — user sessions, object caches
- **Streams**: append-only log — durable message passing (Redis Streams)

## Things to watch out for

- KEYS * is O(n) and blocks the server — never use in production; use SCAN instead
- Large keys can cause slow operations — be aware of collection sizes
- No built-in transactions with rollback; MULTI/EXEC is atomic but not transactional in the RDBMS sense
