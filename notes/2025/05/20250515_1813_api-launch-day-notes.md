---
title: API v2 launch day — notes
slug: api-launch-day-notes
tags: [work, milestone, api]
public: true
---

# API v2 launch day — notes

May 15. Launched at 8:02am PT (two minutes late — the announcement email took longer to send than expected because the mailing list was larger than tested).

## What happened

**First hour:** 47 new signups. Traffic spiked to 3x our baseline within 20 minutes of the announcement. Monitoring held — no alerts fired. The rate limiting (from [[20250305_1754]]) kicked in on a few clients who immediately started hammering the endpoints; they got 429s, which is correct.

**Morning:** Steady stream of signups. A few integration questions in the support Slack. One user found a documentation issue (endpoint URL in one example was wrong — pointed to staging instead of production). Fixed and redeployed the docs within 30 minutes.

**Afternoon:** One actual incident — a caching issue on the /status endpoint was returning stale data for 15 minutes. HTTP caching bug, different from the one fixed in [[20250418_1790]]. Resolved quickly. Post-mortem scheduled.

**End of day:** 214 new signups. All monitoring green. No P1 incidents (the caching issue was P2).

## How it felt

Busy but not frantic. The preparation paid off. The dry run on Friday meant today felt familiar.

The Grafana dashboard (see [[20250405_1779]]) was open all day on a monitor. The SLO tracking from [[20250513_1811]] showed we stayed well within targets even with the traffic spike.

## What I noticed

More developers than expected were coming from directions we hadn't anticipated — a few from communities I didn't know existed. Word of mouth moves faster than I planned for.

Tomorrow: the post-mortem for the caching incident. Then: rest.

Stripe's launch strategy reference: [https://stripe.com/blog/engineering](https://stripe.com/blog/engineering)
