---
title: Q2 planning meeting notes
slug: q2-planning-meeting
tags: [work, planning, meetings]
description: Notes from the Q2 roadmap session, decisions and outstanding items
---

# Q2 planning meeting notes

**Date:** 7 April 2025  
**Attendees:** Petra, Jonas, myself, and Mira joining remotely  
**Duration:** ~90 minutes

---

## Decisions made

**Infrastructure migration** — agreed to move the staging environment to the new provider by end of April. Jonas owns the DNS cutover. Mira flagged that the current SSL cert expires 14 May so this needs to happen before that or we renew on the old provider first.

**API versioning** — we'll deprecate v1 endpoints on 1 July. Three clients still hitting v1 as of last week's logs. Petra to contact them directly rather than sending the generic email.

**Release cadence** — moving from ad-hoc to a two-week cycle starting 21 April. Release notes to be written by whoever owns the changes, not collapsed into a single summary afterward.

## Parked items

- Whether to add a changelog endpoint to the public API — not a Q2 priority, revisit in June
- The monitoring dashboard consolidation — Jonas needs two weeks to scope it properly
- Mobile push notification support — Mira wants clearer user data before committing

## Action items

| Owner | Task | Due |
|-------|------|-----|
| Jonas | DNS migration plan document | 14 Apr |
| Petra | Contact v1 API clients | 11 Apr |
| Me | Draft v1 deprecation notice text | 14 Apr |
| Mira | Pull 30-day push notification engagement data | 18 Apr |

## Reference

The API versioning approach we're following loosely aligns with the [Stripe versioning model](https://stripe.com/blog/api-versioning) — date-based, opt-in upgrades, long deprecation windows.

Next meeting: 5 May, same time.
