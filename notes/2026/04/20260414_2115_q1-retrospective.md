---
title: Q1 retrospective
slug: q1-retrospective
tags: [work, retrospective, quarterly-review]
description: Looking back at the first quarter of 2026 — what shipped, what didn't, what I learned
---

# Q1 retrospective

End of Q1 (technically we're two weeks into April, but Q1 review is appropriate now).

## What shipped

**Onboarding flow**: shipped February 25. Metrics confirmed the hypothesis — trial-to-paid conversion up 8 percentage points on the new flow vs old (68% vs 51% completion rate in [[20260208_2055]]). Best outcome of the quarter.

**Database performance**: the missing index on `events.created_at` went from 4200ms to 12ms on the slow query. Small change, large impact. This is the kind of work that's invisible when done correctly.

**API gateway foundation**: auth and routing working in production. Rate limiting shipped April 10 (see [[20260414_2099]]). This is the beginning of Q2's architectural work, not Q1's.

## What didn't ship

The analytics dashboard: pushed to Q2 per the January planning meeting (see [[20251230_2023]]). Still on the list; the onboarding flow had to come first.

The documentation pass: still incomplete. This is always the thing that gets deferred. Making it a Q2 commitment with an actual calendar block.

## What I learned

**About estimation**: the onboarding flow took 6 weeks from acceptance criteria to production. The original estimate was 4 weeks. The 2-week slip was absorb-able because we'd padded the Q1 plan. Next time: add 30% to engineering estimates as a rule, not a hope.

**About technical decisions**: choosing Proposal C (thin Go proxy) over GraphQL was right for Q1. The simplicity made it shippable. Sophistication later.

**Personal note**

The intentions from [[20260101_2026]] are mostly holding. The reading before phone habit is at about 75%. The cooking projects advanced significantly. The six people were contacted.

Q1 is a foundation. Q2 will build on it.

Retrospective frameworks: https://www.atlassian.com/team-playbook/plays/retrospective
