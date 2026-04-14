---
title: Q1 2025 planning notes
slug: q1-planning-notes
tags: [planning, work, quarterly]
description: Intentions and goals for Q1 2025 at work and personally.
public: true
---

# Q1 2025 planning notes

Setting the frame for the first quarter. Trying to be more intentional about quarterly planning after a fairly reactive Q3 and Q4 last year.

## Work priorities

**API v2 testing and launch**: this carries over from Q4 and needs to close. The external docs and the internal runbook are the two remaining blockers. Target: launch by end of February.

**Database observability**: we have monitoring on infrastructure but not enough on the application layer — slow queries aren't being caught proactively. Plan: implement `pg_stat_statements` properly and set up alerting on queries over 500ms. Reference [[20241211_1678]] on observability.

**Kubernetes migration**: we're moving one service to Kubernetes in Q1 as a learning exercise before migrating the full stack. My notes on the basics [[20241228_1695]] are a starting point; need to go deeper on networking and RBAC.

**Documentation**: the team is growing (two new engineers joining in February). The internal docs need to be in a state where someone can onboard without a guided tour.

## Personal/professional

Continue the intention to write more publicly. I've been journaling privately [[20241231_1698]] and making these notes but haven't published anything technical in over a year. This quarter: write and publish at least one technical piece.

## Learning target

Distributed systems — specifically: consensus algorithms, the CAP theorem in practice, and how message queues actually work under the hood. Leaning on [Martin Kleppmann's Designing Data-Intensive Applications](https://dataintensive.net) and the [Distributed Systems lecture videos](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB) from Cambridge.

## What I'm not doing this quarter

Taking on new side projects. Saying yes to things that don't fit the above. Making the mistake of optimising Q1 planning at the expense of Q1 execution.
