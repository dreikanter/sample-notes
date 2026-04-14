---
title: JSON Schema — practical reference
slug: json-schema-reference
tags: [json, api, reference]
---

# JSON Schema — practical reference

Using JSON Schema for API request/response validation. Notes on the parts I use most.

**Basic types**

```json
{ "type": "string" }
{ "type": "number" }
{ "type": "boolean" }
{ "type": "null" }
{ "type": "array" }
{ "type": "object" }
```

**Object with required properties**

```json
{
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "age": { "type": "integer", "minimum": 0 }
  },
  "required": ["name"],
  "additionalProperties": false
}
```

**Array constraints**

```json
{
  "type": "array",
  "items": { "type": "string" },
  "minItems": 1,
  "maxItems": 10,
  "uniqueItems": true
}
```

**String formats and patterns**

```json
{ "type": "string", "format": "email" }
{ "type": "string", "format": "date" }
{ "type": "string", "pattern": "^[a-z0-9-]+$" }
{ "type": "string", "minLength": 1, "maxLength": 100 }
```

**Enum**

```json
{ "type": "string", "enum": ["active", "inactive", "pending"] }
```

**Composition**

```json
{ "oneOf": [{ "type": "string" }, { "type": "number" }] }
{ "anyOf": [...] }    /* at least one must match */
{ "allOf": [...] }    /* all must match */
{ "not": { "type": "null" } }
```

**`$ref` for reuse**

```json
{
  "$defs": {
    "Address": { "type": "object", "properties": { ... } }
  },
  "properties": {
    "billing": { "$ref": "#/$defs/Address" }
  }
}
```

Docs: [json-schema.org](https://json-schema.org/learn/getting-started-step-by-step)
