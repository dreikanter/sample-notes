---
title: Redis caching patterns
slug: redis-caching-patterns
tags: [redis, caching, backend, performance]
description: Common Redis caching patterns with tradeoffs
---

# Redis caching patterns

Notes from implementing caching on the rate-limiting project and reading through various postmortems.

## Cache-aside (lazy loading)

The most common pattern. Application checks cache first; on miss, fetches from DB and populates cache.

```javascript
async function getUser(id) {
  const cacheKey = `user:${id}`;
  const cached = await redis.get(cacheKey);
  if (cached) return JSON.parse(cached);

  const user = await db.users.findById(id);
  await redis.setex(cacheKey, 3600, JSON.stringify(user));
  return user;
}
```

Tradeoff: cache is cold until data is requested. First request after a deploy hits the DB.

## Write-through

Write to cache and DB simultaneously on every write. Cache is never stale but every write is slower.

Good for: data that's read frequently after being written, situations where stale data is unacceptable.
Bad for: write-heavy workloads where cached data is rarely read.

## TTL design

The hardest part of caching is choosing TTL. Too short: cache doesn't help. Too long: stale data causes bugs.

General heuristics:
- User session data: 30 minutes, refreshed on activity
- Reference data (country codes, etc.): 24 hours
- User profiles: 15-60 minutes depending on update frequency
- Computed aggregates: match your reporting cadence

## Cache stampede prevention

When a popular key expires, many processes simultaneously fetch from DB. Solution:

```javascript
// Probabilistic early expiration
// Starts refreshing before TTL expires, probabilistically
const remaining = await redis.ttl(key);
const threshold = remaining < 30 && Math.random() < 0.1;
if (!cached || threshold) {
  // refetch and repopulate
}
```

Or use a locking pattern with `SET key value NX PX timeout`.

[Redis documentation on patterns](https://redis.io/docs/latest/develop/use/patterns/)
