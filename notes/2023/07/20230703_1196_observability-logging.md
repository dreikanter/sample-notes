---
title: Structured logging — practical setup
slug: observability-logging
tags: [observability, logging, backend]
description: Notes on structured logging setup across Python and Node services.
---

# Structured logging — practical setup

Consolidating notes from setting up structured logging across the main services.

**Why structured logging**

Plain text logs are human-readable but machine-hostile. If every log line is JSON, you can filter, aggregate, and alert on specific fields without regex. `level:"error" service:"notifications" user_id:"123"` is a query, not a guess.

**Python setup (structlog)**

```python
import structlog

log = structlog.get_logger()

log.info("notification.sent",
    user_id=user.id,
    channel="email",
    notification_type="weekly_digest",
    duration_ms=elapsed)
```

Configure in app startup:

```python
structlog.configure(
    processors=[
        structlog.stdlib.add_log_level,
        structlog.stdlib.add_logger_name,
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.JSONRenderer()
    ]
)
```

**Node/TypeScript setup (pino)**

```typescript
import pino from 'pino';
const log = pino({ level: process.env.LOG_LEVEL || 'info' });

log.info({ userId, eventType, durationMs }, 'event processed');
```

**What to always include**

- `level` — obvious
- `service` — which service produced this log
- `trace_id` / `request_id` — for correlating across services
- `timestamp` — ISO 8601
- Relevant entity IDs (`user_id`, `order_id`, etc.)

**What not to log**

Passwords, tokens, PII (email addresses in plain text, full addresses). Sanitise before logging.

**Aggregation**

We're using Loki + Grafana. Alternatives: Datadog, Elasticsearch. The structured format means migration between backends is feasible.

[structlog documentation](https://www.structlog.org/en/stable/)
