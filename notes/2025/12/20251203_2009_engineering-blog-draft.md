---
title: Engineering blog draft — feature flag rollout patterns
slug: engineering-blog-draft
tags: [work, writing, engineering, draft]
---

# Engineering blog draft — feature flag rollout patterns

Draft of the engineering blog post Thomas asked about. See the completed rollout at [[20251128_2006]].

---

## Gradual rollouts without the anxiety: what we learned

Deploying new features to production is one of the more reliably stressful parts of software engineering. Even with good tests and staging environments, production has a way of finding problems that didn't exist anywhere else.

Feature flags—the ability to enable a feature for a percentage of users without a code deployment—have become standard practice. What's less commonly documented is how to use them well. Here's what we learned from a recent API migration.

### The pattern

We use a stable per-user bucketing approach: a SHA-256 hash of the flag name and user ID, modulo 100, gives us a consistent bucket for each user. "Consistent" matters: the same user should always be in or out of a rollout for a given flag; flipping between requests makes debugging much harder.

The flag configuration lives in Redis so we can change the rollout percentage without a deploy:

```python
def is_in_rollout(user_id, flag_name, percentage):
    key = f"{flag_name}:{user_id}"
    bucket = int(hashlib.sha256(key.encode()).hexdigest(), 16) % 100
    return bucket < percentage
```

### What this bought us

We found a real production bug at 5% exposure that we would have caught at 100% instead—affecting 1 in 20 users rather than all of them. The fix was a missing database migration, deployed in under two hours.

### The non-obvious benefits

Beyond catching bugs at lower impact, incremental rollouts change the emotional experience of shipping. When you know you can stop at 5%, the fear that prevents shipping dissolves.

---

**Status:** First draft, needs editing. Share with Thomas before publishing.

Reference: [[20251023_1978]] for the implementation details.
