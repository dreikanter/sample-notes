# Redis caching patterns reference

Working notes from the read replica investigation. Need to understand what's currently cached to plan what happens when the database layer changes.

## Cache-aside (lazy loading)

```python
def get_user(user_id: str) -> User:
    cached = redis.get(f"user:{user_id}")
    if cached:
        return User.from_json(cached)
    user = db.query(User).filter_by(id=user_id).first()
    redis.setex(f"user:{user_id}", 300, user.to_json())  # 5 minute TTL
    return user
```

The cache is only populated when data is read. Cold cache means direct database hits until the cache warms. Good for data that isn't accessed uniformly.

## Write-through

On every write to the database, also write to the cache. Keeps cache warm. Costs a write for data that may never be read. We use this for the live counters in the event pipeline (see [[20231212_1337]]).

## Cache invalidation strategies

- **TTL-based:** Simple, tolerates some staleness. Good for read-heavy, low-change data.
- **Event-driven invalidation:** Publish a "user updated" event, invalidate cache on receipt. More complex plumbing but tighter consistency.
- **Versioned keys:** `user:{user_id}:v{version}`. The old key becomes inaccessible, not deleted. Natural expiry via TTL. Useful when the stale key is actively harmful.

## Common mistakes I've seen

- No TTL: cache grows indefinitely, memory pressure builds, eviction policy kicks in randomly
- Caching mutable aggregates: cache the `user_order_count` but not the individual orders — then an order update invalidates nothing
- Not serializing consistently: different serialization in different code paths causes deserialization errors

Reference: [Redis documentation on caching patterns](https://redis.io/docs/latest/develop/use/patterns/) and [Martin Fowler's bliki on caching](https://martinfowler.com/bliki/).
