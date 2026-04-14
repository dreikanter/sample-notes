---
title: Performance profiling — finding the actual bottleneck
slug: performance-profiling
tags: [performance, programming, devops]
description: Notes on systematic performance investigation
---

# Performance profiling — finding the actual bottleneck

Just finished a week of performance investigation on a slow API endpoint. Notes on the process that turned out to matter.

**The actual problem was not what we thought**

Initial hypothesis: the slow queries. We had a query that took 200ms and felt like the obvious culprit. Added an index, it went to 15ms. The endpoint was still slow.

The real bottleneck: N+1 queries. A list endpoint that returned 50 records was making 51 database queries — one for the list, then one per record to fetch a related resource that wasn't being eagerly loaded. The fix was one line: adding `.prefetch_related()` to the queryset. Query count went from 51 to 2.

**The process that worked**

1. Measure before doing anything. "It feels slow" is not a baseline. We established: P50 latency, P95 latency, request count, query count.
2. Find the actual constraint (CPU? I/O? network? database?). In our case: database. Established this by looking at time spent in the profiler.
3. Profile, don't guess. Used Django Debug Toolbar in development to see exactly which queries were executing and how many times.
4. Fix the largest contributor first.
5. Measure again after each change to confirm improvement.

**What we improved overall**

- 51 queries → 2 queries (prefetch)
- Added missing index on a FK column used in filtering
- Added response caching for a reference data endpoint (almost never changes)
- P95 went from 1.8s to 180ms

**Lesson**

Premature optimization is the root of all evil, but premature diagnosis is a close second. The index we added first was not wasted work, but it wasn't the bottleneck.

Django performance guide: https://docs.djangoproject.com/en/stable/topics/performance/
