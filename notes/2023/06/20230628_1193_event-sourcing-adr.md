---
title: ADR — event sourcing in the notification service
slug: event-sourcing-adr
tags: [architecture, adr, work]
description: Architecture decision record for the event sourcing approach used in Q1.
---

# ADR — event sourcing in the notification service

**Status:** Accepted (Q1 2023, documented retroactively)

**Context**

The notification service needed to support: replay of notifications for debugging, audit trail of what was sent to whom and when, ability to query notification history without hitting the primary delivery system, and eventual support for multiple delivery channels from a single event stream.

A traditional CRUD approach (create/update notification records) would have required significant denormalisation for the audit trail and would have made replay complex.

**Decision**

Use event sourcing for the notification service. Each notification lifecycle (created, scheduled, delivered, failed, retried) is represented as an immutable event appended to an event log. Current state is derived from replaying the event log.

**Consequences**

*Positive:*
- Audit trail is inherent to the approach, not bolted on
- Replay is trivial
- The event stream can be consumed by other services (analytics, admin tools) without coupling to the notification service's internals

*Negative:*
- Query patterns are harder — you can't easily query "all undelivered notifications" without a projection
- Schema evolution of events is more complex than schema evolution of tables
- The team needed time to internalise the mental model

**What we'd change**

We underinvested in projections at the start. The event log alone is hard to work with for operational queries — we should have built read-model projections earlier.

See [Martin Fowler on event sourcing](https://martinfowler.com/eaaDev/EventSourcing.html) for the pattern reference.
