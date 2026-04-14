---
title: API design sync — June 1
slug: api-design-meeting
tags: [work, meetings, api]
description: Notes from the event schema API design discussion
---

# API design sync — June 1

**Attendees:** Priya, Sam, Theodora, me
**Duration:** 50 minutes

## Context

We've been going back and forth on the event schema RFC (see the week 21 notes at [[20240520_1052]]). This meeting was meant to resolve the timestamp ambiguity question and close out the open comments.

## What we decided

**Timestamps:** All timestamps in the API will be ISO 8601 with explicit UTC offset — no bare epoch integers, no ambiguous local times. The `created_at` and `updated_at` fields will always be present; `event_at` is optional for events that have a meaningful occurrence time distinct from creation.

**Versioning:** Media-type versioning (`application/vnd.app.v2+json`) rather than URL path versioning. Theodora made the strongest argument: path versioning leaks into caches and makes routing logic weird. We'll keep the URL structure clean.

**Pagination:** Cursor-based only. No offset pagination in the new schema. Sam pushed back on implementation complexity but the consensus was that offset pagination breaks predictably under writes and we'd regret it.

## Still open

- Error envelope format: Priya wants to follow [RFC 9457 (Problem Details)](https://www.rfc-editor.org/rfc/rfc9457) exactly; Sam wants a simplified version. Tabled to async.
- Rate limiting headers: do we expose `X-RateLimit-*` or `RateLimit-*` (the IETF draft)? Leaning toward the IETF draft since it's closer to standardization.

## Action items

- Me: Update the RFC with the timestamp and versioning decisions by June 5
- Priya: Write up the error envelope options with concrete examples
- Sam: Prototype the cursor pagination in the staging environment
