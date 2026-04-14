---
title: System design — notes on rate limiting approaches
slug: system-design-notes
tags: [system-design, architecture, programming]
description: Comparison of rate limiting algorithms
---

# System design — notes on rate limiting approaches

Working on rate limiting for an API. Made notes on the different algorithm options.

**Token bucket**

Each client gets a bucket that fills with tokens at a fixed rate. Each request consumes one token. If no tokens are available, the request is rejected. Allows bursting: if a client hasn't used its allocation, tokens accumulate up to the bucket's capacity, and they can all be spent at once.

Suitable when you want to allow short bursts but control average throughput.

**Leaky bucket**

Requests enter a queue (the bucket) and are processed at a fixed rate. If the queue is full, new requests are dropped. Smooths out bursts — the output rate is constant regardless of input rate.

More suitable for traffic shaping than rate limiting, because bursty requests don't fail fast — they queue.

**Fixed window counter**

Count requests per time window (per minute, per hour). Simple to implement; vulnerable to the boundary problem: a client can make double the allowed requests by hitting the limit at the end of one window and immediately at the start of the next.

**Sliding window log**

Maintain a log of request timestamps. On each request, remove timestamps older than the window, count remaining, compare to limit. Accurate but memory-intensive at scale (storing all timestamps per client).

**Sliding window counter**

Approximation of sliding window using two fixed window counters. `current_window_count + previous_window_count * overlap_fraction`. Much more memory efficient, slight inaccuracy acceptable in practice.

Redis for distributed rate limiting: https://redis.io/docs/manual/patterns/rate-limiting/
