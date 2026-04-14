# PostgreSQL JSON operators — working notes

Quick reference for JSONB queries, assembled while building a filter interface that needed to reach into nested document fields without a schema migration.

## Operators

| Operator | Meaning |
|---|---|
| `->` | Get JSON object field (returns JSON) |
| `->>` | Get JSON object field as text |
| `#>` | Get at path (array of keys) |
| `#>>` | Get at path as text |
| `@>` | Does left contain right? |
| `<@` | Is left contained in right? |
| `?` | Does key exist? |
| `?|` | Do any of these keys exist? |
| `?&` | Do all of these keys exist? |

## Examples

```sql
-- Get nested field
SELECT payload->>'status' FROM events;

-- Filter by nested value
SELECT * FROM events WHERE payload @> '{"status": "active"}';

-- Path lookup
SELECT payload #>> '{user,email}' FROM events;

-- Index for containment queries (essential for performance)
CREATE INDEX idx_events_payload ON events USING GIN (payload);
```

## Notes on jsonb vs json

Always use `jsonb` unless you have a specific reason not to. It stores parsed binary, enables indexing, and supports all the operators above. `json` stores raw text and is mainly useful if you need to preserve key order or exact whitespace (rare).

GIN index is the right default for containment and key-existence queries. For path-based queries used repeatedly, a functional index on the extracted value can be faster.

Full operator reference: https://www.postgresql.org/docs/current/functions-json.html

For the migration approach that added these columns without downtime, see [[20240214_1042]].
