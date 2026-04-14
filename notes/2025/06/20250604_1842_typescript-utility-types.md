---
title: TypeScript utility types — practical examples
slug: typescript-utility-types
tags: [typescript, programming, reference]
description: Practical usage notes for TypeScript's built-in utility types
public: true
---

# TypeScript utility types — practical examples

The standard library utility types are well-documented but easy to misuse or underuse. These are the ones I reach for regularly.

**Partial and Required**

```typescript
interface Config {
  host: string;
  port: number;
  debug: boolean;
}

// All fields optional — useful for update payloads
function updateConfig(changes: Partial<Config>) { ... }

// All fields required — useful when all optional fields need to be present
function validateConfig(config: Required<Config>) { ... }
```

**Pick and Omit**

```typescript
type UserPreview = Pick<User, 'id' | 'name' | 'avatarUrl'>;
type CreateUserPayload = Omit<User, 'id' | 'createdAt' | 'updatedAt'>;
```

Omit is usually cleaner than Pick when you want "everything except a few system fields."

**Record**

```typescript
type StatusMap = Record<'pending' | 'active' | 'archived', number>;
// Enforces that all union members are present as keys
```

**ReturnType and Parameters**

```typescript
type FetchResult = ReturnType<typeof fetchUser>;
type FetchArgs = Parameters<typeof fetchUser>;
```

Useful when you need to type variables that hold function return values without repeating the type definition.

**NonNullable**

```typescript
type SafeId = NonNullable<string | null | undefined>; // string
```

**Conditional application**

```typescript
type ReadonlyIf<T, Readonly extends boolean> =
  Readonly extends true ? Readonly<T> : T;
```

Full reference: https://www.typescriptlang.org/docs/handbook/utility-types.html
