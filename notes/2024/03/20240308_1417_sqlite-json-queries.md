# SQLite JSON queries

Quick reference for querying JSON columns in SQLite 3.38+. The JSON functions have been stable since 3.9 but the `->>` operator shorthand only appeared in 3.38.

## Extracting values

```sql
-- Old style
SELECT json_extract(data, '$.name') FROM events;

-- New shorthand (3.38+)
SELECT data->>'name' FROM events;
SELECT data->'tags'->0 FROM events;  -- first element of array
```

## Filtering on JSON fields

```sql
SELECT * FROM events
WHERE data->>'status' = 'active'
  AND json_extract(data, '$.priority') > 2;
```

## JSON array operations

```sql
-- Count elements
SELECT json_array_length(data->'tags') FROM events;

-- Check if array contains value
SELECT * FROM events
WHERE EXISTS (
  SELECT 1 FROM json_each(data->'tags')
  WHERE value = 'urgent'
);
```

## Building JSON

```sql
SELECT json_object('id', id, 'name', name, 'ts', created_at)
FROM users LIMIT 5;

SELECT json_group_array(name) FROM users WHERE active = 1;
```

## Practical note

SQLite won't validate JSON on insert unless you add a CHECK constraint:

```sql
CREATE TABLE events (
  id INTEGER PRIMARY KEY,
  data TEXT CHECK(json_valid(data))
);
```

Without the constraint, malformed JSON silently breaks queries. Found this the hard way on a project where a Python dict got serialized with single quotes instead of double.

Full documentation: https://www.sqlite.org/json1.html
