# Interview prep notes — backend engineering

Preparing to interview candidates for the open backend role (from Q4 plan, see [[20250925_1950]]). Writing down what I want to assess and how.

## What I'm looking for

**Core competency:** Can they reason about systems under load? Not whether they've memorized specific algorithms, but whether they can think through tradeoffs.

**Collaboration signals:** How do they discuss past work? Do they say "I built" or "we built"? Do they attribute failures to circumstances or reflect on their own choices?

**Communication:** Can they explain a complex thing simply? Can they say "I don't know" without crumbling?

## Question structure

**Opening (5 min):** Walk me through something you built that you're proud of. (Tells me a lot about where their interests and values are.)

**Technical depth (25 min):**

1. Database question: given a large table with millions of rows and a slow query, how do you diagnose and fix it? (Looking for: EXPLAIN ANALYZE, indexing strategy, query restructuring, knowing when not to index.)

2. Distributed systems question: how do you handle a downstream service that starts returning errors intermittently? (Looking for: circuit breaker, retry with backoff, idempotency, graceful degradation.)

3. Debugging scenario: a service's memory usage grows linearly over 6 hours, then crashes. Walk me through your investigation. (Looking for: systematic approach, profiling, logging, hypothesis-testing.)

**Collaboration/process (15 min):** Tell me about a time you disagreed with a technical decision. What happened?

**Their questions (15 min):** These tell me as much as the answers.

Reference: [Hiring Engineers by Stripe Press](https://press.stripe.com/) had useful framing, though aimed at early-stage companies.

