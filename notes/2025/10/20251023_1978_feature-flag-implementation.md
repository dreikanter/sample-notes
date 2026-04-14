---
title: Feature flag implementation — rollout pattern
slug: feature-flag-implementation
tags: [engineering, redis, backend, patterns]
---

# Feature flag implementation — rollout pattern

Documented the approach used for the notification preferences rollout (referenced in [[20251021_1976]]). Simple percentage-based rollout with stable per-user assignment.

## The problem

We want to roll out a new notification API to a percentage of users—say 5% initially—and have each user stay in or out of the rollout consistently (not flipping each request).

## The solution

Use a stable hash of the user ID modulo 100 to determine bucket assignment:

```python
import hashlib

def is_in_rollout(user_id: int, flag_name: str, percentage: float) -> bool:
    """
    Returns True if user is in the rollout.
    Stable: same user always gets same result for same flag/percentage.
    """
    key = f"{flag_name}:{user_id}"
    hash_val = int(hashlib.sha256(key.encode()).hexdigest(), 16)
    bucket = hash_val % 100
    return bucket < percentage
```

## Storing the flag config in Redis

```python
import redis
import json

r = redis.Redis()

def get_flag(flag_name: str) -> dict:
    raw = r.get(f"flag:{flag_name}")
    if raw is None:
        return {"enabled": False, "percentage": 0}
    return json.loads(raw)

def set_flag(flag_name: str, enabled: bool, percentage: float):
    r.set(
        f"flag:{flag_name}",
        json.dumps({"enabled": enabled, "percentage": percentage})
    )
```

## Usage

```python
def get_notification_api_version(user_id: int) -> str:
    flag = get_flag("notification_v2")
    if flag["enabled"] and is_in_rollout(user_id, "notification_v2", flag["percentage"]):
        return "v2"
    return "v1"
```

## Properties

- **Stable:** Same user always in same bucket; deterministic from user_id
- **Gradual:** Increase percentage in Redis without deploys
- **Independent flags:** flag_name prefix ensures different flags don't correlate

Reference: [LaunchDarkly's explanation of bucketing](https://docs.launchdarkly.com/sdk/concepts/variation-calls) for a more complete treatment.
