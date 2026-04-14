---
title: PostgreSQL JSONB — working notes
slug: postgres-jsonb-notes
tags: [postgres, sql, reference]
---

# PostgreSQL JSONB — working notes

JSONB stores JSON in a decomposed binary format, making it faster to process than `json` (which stores the raw text). Indexed queries work well. Worth using when the structure is partially dynamic but you still need to query inside it.

## Key operators

```sql
-- Access object field (returns JSON)
data -> 'key'

-- Access object field (returns text)
data ->> 'key'

-- Access nested path
data #> '{a,b,c}'
data #>> '{a,b,c}'

-- Check if key exists
data ? 'key'

-- Check if any keys exist
data ?| array['key1', 'key2']

-- Check if all keys exist
data ?& array['key1', 'key2']

-- Contains (left contains right)
data @> '{"status": "active"}'

-- Is contained by
data <@ '{"status": "active", "role": "admin"}'
```

## Indexing

GIN index supports `@>`, `?`, `?|`, `?&`:

```sql
CREATE INDEX idx_data_gin ON records USING gin(data);
```

For specific path access, use expression indexes:

```sql
CREATE INDEX idx_status ON records ((data ->> 'status'));
```

## Querying arrays in JSONB

```sql
-- Any element in array equals value
data @> '{"tags": ["billing"]}'

-- jsonb_array_elements to unnest
SELECT id FROM records, jsonb_array_elements_text(data->'tags') AS tag
WHERE tag = 'billing';
```

Official docs: [https://www.postgresql.org/docs/current/datatype-json.html](https://www.postgresql.org/docs/current/datatype-json.html)
