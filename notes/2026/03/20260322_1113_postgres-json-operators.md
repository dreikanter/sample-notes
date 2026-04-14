# PostgreSQL JSON and JSONB operators

Quick reference for the operators I use most often when working with JSONB columns. Full docs at https://www.postgresql.org/docs/current/functions-json.html.

## Extraction operators

```sql
-- Arrow: returns JSON
SELECT data -> 'name' FROM users;          -- {"first": "Ada"}
SELECT data -> 'scores' -> 0 FROM users;   -- first array element

-- Double arrow: returns text
SELECT data ->> 'name' FROM users;         -- Ada (no quotes)

-- Path operator (JSON)
SELECT data #> '{address, city}' FROM users;

-- Path operator (text)
SELECT data #>> '{address, city}' FROM users;
```

## Containment and existence

```sql
-- Does left side contain right?
SELECT * FROM users WHERE data @> '{"role": "admin"}';

-- Does key exist?
SELECT * FROM users WHERE data ? 'email';

-- Does any key from array exist?
SELECT * FROM users WHERE data ?| ARRAY['phone', 'email'];

-- Do all keys from array exist?
SELECT * FROM users WHERE data ?& ARRAY['name', 'email'];
```

## Updating JSONB

```sql
-- Replace a key
UPDATE users SET data = jsonb_set(data, '{name}', '"Beatrice"');

-- Remove a key
UPDATE users SET data = data - 'temp_field';

-- Remove by path
UPDATE users SET data = data #- '{address, zip}';

-- Concatenate / merge (right side wins on conflict)
UPDATE users SET data = data || '{"verified": true}';
```

## Indexing

```sql
-- GIN index for containment and key existence
CREATE INDEX idx_users_data ON users USING GIN (data);

-- Partial index on specific key
CREATE INDEX idx_users_role ON users USING GIN ((data -> 'role'));
```

## Gotchas

- `->` returns JSON, `->>` returns text. Comparisons with `=` on `->>` do string comparison, not type-aware.
- JSONB stores deduplicated keys; JSON preserves original order and duplicates.
- `jsonb_set` does not create nested paths — the intermediate keys must already exist.
- For bulk updates, `jsonb_each` is useful to unpack and re-aggregate.

Came up while debugging a query related to the Docker config work in [[20250830_1012]] — the compose environment variables were being stored as JSONB in a metadata column.
