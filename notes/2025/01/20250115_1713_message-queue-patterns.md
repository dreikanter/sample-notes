---
title: Message queue patterns — practical notes
slug: message-queue-patterns
tags: [messaging, backend, distributed-systems, rabbitmq]
description: Working notes on message queue patterns with RabbitMQ and Redis.
---

# Message queue patterns — practical notes

Notes from reading and implementation, part of the distributed systems focus for Q1 [[20250112_1710]]. Reference: [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials/).

## Why message queues

Decouple producers from consumers. Handle load spikes (buffer work when consumers are slow). Enable retry on failure. Allow multiple consumers for the same work type. Provide durability — messages survive consumer restarts.

## Core patterns

**Work queue (competing consumers)**: Multiple workers pull from the same queue. Work distributed round-robin. Good for: task processing, email sending, report generation.

```
Producer → Queue ← Consumer 1
                 ← Consumer 2
                 ← Consumer 3
```

**Publish/Subscribe**: Publisher sends to an exchange; exchange routes to multiple queues. Good for: notifications, cache invalidation, event broadcasting.

**Dead letter queue**: Messages that fail processing (after N retries) are routed to a separate queue for inspection. Essential for production systems.

## Delivery guarantees

- **At-most-once**: send and forget, no retry. Risk: message loss.
- **At-least-once**: retry until acknowledged. Risk: duplicate processing.
- **Exactly-once**: complex, expensive, usually not worth it. Design consumers to be idempotent instead.

**Designing for idempotency**: include a unique message ID; check if already processed before acting; use upsert patterns in the database. Most systems are more robust at at-least-once + idempotent consumers than at-exactly-once.

## RabbitMQ vs Redis Streams vs Kafka

| | RabbitMQ | Redis Streams | Kafka |
|---|---|---|---|
| Throughput | Medium | High | Very high |
| Persistence | Yes (with disk) | Yes | Yes |
| Consumer groups | Yes | Yes | Yes |
| Complexity | Medium | Low | High |

Use Redis Streams for moderate throughput with simple operational requirements. RabbitMQ for flexible routing. Kafka when you need very high throughput or event log replay.

## Practical gotchas

- Acknowledge after processing, not before — otherwise messages are lost on consumer crash
- Set message TTL and queue limits to prevent unbounded growth
- Monitor queue depth — growing queues indicate consumers are falling behind
