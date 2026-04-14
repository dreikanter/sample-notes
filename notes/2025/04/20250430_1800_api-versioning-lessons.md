---
title: API versioning — lessons from the v1 to v2 migration
slug: api-versioning-lessons
tags: [api, work, architecture]
---

# API versioning — lessons from the v1 to v2 migration

With the v2 beta complete and public launch approaching, writing up what I'd do differently.

## What worked

**URL versioning** (`/v1/`, `/v2/`) is simple for clients to understand and easy to route. Some engineers prefer header versioning (`Accept: application/vnd.myapi.v2+json`) but URL versioning won. The pragmatic argument: URLs are logged, can be bookmarked, are visible in browser dev tools. Header versioning is invisible.

**Long deprecation window:** Announcing v1 deprecation in March for a June 30 end-of-life gave clients 3.5 months. We've had almost no complaints about the timeline.

**Parallel operation:** Both versions ran simultaneously from day one of the beta. This let some beta users stay on v1 while testing v2 in parallel.

## What I'd do differently

**Sandbox environment earlier.** Beta users asked for this on day one. A free-tier sandbox with reset-on-demand would have reduced the friction of early testing significantly.

**Tighter error contracts.** The inconsistent error format between v1 and v2 (flagged by a beta user and in the retrospective [[20250415_1787]]) should have been caught in design review. Need a standard error schema before writing any endpoints.

**Client library alongside the API.** We shipped the API, then started the client library 4 weeks later. The sequence should be parallel — the library design often reveals API contract problems before external clients hit them.

**Never expose internal implementation details.** v1 had some field names that mapped directly to database column names (e.g., `created_ts` instead of `createdAt`). These are embarrassing in documentation and lock the implementation.

Stripe's API changelog is still the reference design: [https://stripe.com/docs/upgrades](https://stripe.com/docs/upgrades)
