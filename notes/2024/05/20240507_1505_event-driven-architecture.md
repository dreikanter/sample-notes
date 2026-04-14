---
title: Event-driven architecture — working notes
slug: event-driven-architecture
tags: [architecture, events, messaging, programming]
description: Notes on event-driven patterns and when they apply
---

# Event-driven architecture — working notes

Working on a redesign of the notification system. Evaluating event-driven patterns.

## Core concepts

**Event:** Something that happened. Immutable, past tense. `OrderPlaced`, `PaymentFailed`, `UserSignedUp`. Contains the relevant data at the time of occurrence.

**Command:** A request to do something. `PlaceOrder`, `ProcessPayment`. May fail or be rejected.

**Event broker:** The message bus — Kafka, RabbitMQ, AWS SNS/SQS, etc.

**Producer:** Emits events when state changes.
**Consumer:** Subscribes to events, takes action.

## Patterns

**Event notification:** Services emit events when things change; other services update themselves. Loose coupling — the producer doesn't know who's listening.

**Event-carried state transfer:** Events contain enough data for consumers to update their own state without querying the source. Reduces coupling further but events become larger.

**Event sourcing:** The system state is derived from a log of events rather than stored directly. Very powerful for audit trails and temporal queries, complex to implement correctly.

**CQRS (Command Query Responsibility Segregation):** Separate models for reads and writes. Often combined with event sourcing.

## Trade-offs

Gains:
- Loose coupling between services
- Natural audit log
- Services can replay events to rebuild state
- Scales well

Costs:
- Eventual consistency (consumers may lag)
- Debugging is harder (distributed causality)
- Event schema evolution needs care
- Testing is more complex

## When to use

Good fit: multiple services need to react to the same occurrence, ordering/notification/analytics pipelines, audit requirements, services with different scaling needs.

Poor fit: simple CRUD applications, when strong consistency is required, when the team doesn't have experience with distributed systems.

Fowler on event-driven: https://martinfowler.com/articles/201701-event-driven.html
