# SQLite JSON functions cheatsheet

SQLite has had JSON support since 3.9.0 (2015) and it's more capable than most people realise. Quick reference for the functions I use most.

**Extracting values**

```sql
SELECT json_extract(data, '$.name') FROM events;
SELECT json_extract(data, '$.tags[0]') FROM events;
```

**Checking existence**

```sql
SELECT * FROM events WHERE json_extract(data, '$.published') = 1;
```

**Building JSON**

```sql
SELECT json_object('id', id, 'name', name) FROM users;
SELECT json_array(1, 2, 'three');
```

**Aggregating into array**

```sql
SELECT json_group_array(name) FROM tags WHERE active = 1;
SELECT json_group_object(key, value) FROM settings;
```

**Iterating with json_each**

```sql
SELECT key, value FROM json_each('{"a":1,"b":2}');
SELECT value FROM json_each('[10,20,30]');
```

**Patching / merging**

```sql
SELECT json_patch('{"a":1}', '{"b":2}'); -- {"a":1,"b":2}
```

Full documentation at [sqlite.org/json1.html](https://www.sqlite.org/json1.html). The `->` and `->>` operators (PostgreSQL-style shorthand) are available from SQLite 3.38.0 onward and are often cleaner than `json_extract`.
