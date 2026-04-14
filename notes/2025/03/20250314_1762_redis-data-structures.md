---
title: Redis data structures reference
slug: redis-data-structures
tags: [redis, reference, databases]
---

# Redis data structures reference

Quick reference for Redis data types and when to use them. Mostly used for caching and rate limiting at work.

## String

The simplest type. Binary-safe, max 512MB.

```bash
SET key value EX 3600   # set with 60 min TTL
GET key
INCR counter            # atomic increment
INCRBY counter 5
SETNX key value         # set only if not exists
```

## Hash

Field-value pairs within a single key. Good for storing objects.

```bash
HSET user:1 name "Alice" email "alice@example.com" age 30
HGET user:1 name
HGETALL user:1
HINCRBY user:1 login_count 1
HDEL user:1 age
```

## List

Ordered, allows duplicates. Good for queues and stacks.

```bash
RPUSH queue task1 task2   # push to right (tail)
LPUSH queue task0         # push to left (head)
LRANGE queue 0 -1         # all items
LPOP queue                # pop from left
BLPOP queue 30            # blocking pop, 30s timeout
```

## Set

Unordered, unique members.

```bash
SADD tags python redis databases
SMEMBERS tags
SISMEMBER tags python    # 1 if member
SCARD tags               # count
SINTER set1 set2         # intersection
SUNION set1 set2         # union
```

## Sorted set

Members with scores. Used for leaderboards, rate limiting.

```bash
ZADD scores 100 alice 90 bob 95 carol
ZRANGE scores 0 -1 WITHSCORES  # ascending
ZREVRANGE scores 0 2           # top 3 descending
ZSCORE scores alice
ZRANK scores bob
```

## Stream (Redis 5+)

Append-only log structure. Good for event sourcing.

Official docs: [https://redis.io/docs/data-types/](https://redis.io/docs/data-types/)
