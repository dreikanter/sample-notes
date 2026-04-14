# Event pipeline — architecture documentation

Finally writing this (promised in [[20231126_1325]]). The event pipeline has been running without documentation for eight months and two people have already made changes that created subtle ordering bugs because they didn't understand the assumptions.

## Overview

Events flow from producer services → Kafka → pipeline consumer → PostgreSQL (events table) + Redis (live counters).

The pipeline consumer is a single Go service. It runs three goroutines: one for consuming from Kafka, one for batch-inserting to Postgres, and one for updating Redis counters. The goroutines communicate over buffered channels.

## Critical invariants

1. **Events must be inserted to Postgres before Redis is updated.** If the process crashes between the two, the Redis counter is wrong but Postgres is authoritative. At startup, the consumer reconciles Redis from Postgres for the most recent time window.

2. **Kafka consumer group offset commit happens only after successful Postgres insert.** This means a failed batch will be retried. The batch insert is idempotent on `event_id` (unique constraint), so duplicate processing is safe.

3. **Batch size matters for throughput.** Current setting is 500 events per batch. Below 200, Postgres insert latency dominates. Above 1000, memory pressure on the consumer increases noticeably during traffic spikes.

## Configuration

```
KAFKA_BROKERS=broker1:9092,broker2:9092
KAFKA_TOPIC=events
KAFKA_GROUP=pipeline-consumer
PG_BATCH_SIZE=500
REDIS_EXPIRY_HOURS=72
```

## Failure modes observed

- Kafka broker restart causes consumer reconnect delay (~8s). Events queue in Kafka; pipeline catches up within two minutes.
- Postgres primary failover: consumer loses connection, retries with backoff, reconnects to new primary. No data loss.

Reference: [Confluent's guide to Kafka consumer groups](https://docs.confluent.io/platform/current/clients/consumer.html).
