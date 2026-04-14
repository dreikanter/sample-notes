---
title: TypeScript utility types reference
slug: typescript-utility-types
tags: [typescript, reference, frontend]
---

# TypeScript utility types reference

Built-in utility types I reach for frequently. The full list is in the docs but these are the ones that come up in day-to-day work.

## Structural transformations

```typescript
// Make all properties optional
type A = Partial<User>

// Make all properties required
type B = Required<User>

// Make all properties readonly
type C = Readonly<User>

// Pick a subset of properties
type D = Pick<User, 'id' | 'email'>

// Remove properties
type E = Omit<User, 'password' | 'salt'>
```

## Record

```typescript
// Map keys to values
type StatusMap = Record<'active' | 'inactive' | 'pending', number>
// equivalent to { active: number; inactive: number; pending: number }
```

## Conditional types and extraction

```typescript
// Exclude union members
type F = Exclude<'a' | 'b' | 'c', 'a'>  // 'b' | 'c'

// Extract matching union members
type G = Extract<'a' | 'b' | number, string>  // 'a' | 'b'

// Remove null and undefined
type H = NonNullable<string | null | undefined>  // string

// Return type of a function
type I = ReturnType<typeof myFunction>

// Parameter types as tuple
type J = Parameters<typeof myFunction>

// Instance type of a constructor
type K = InstanceType<typeof MyClass>
```

## Template literal types

```typescript
type EventName = `on${Capitalize<string>}`
// matches 'onClick', 'onChange', etc.
```

Official handbook: [https://www.typescriptlang.org/docs/handbook/utility-types.html](https://www.typescriptlang.org/docs/handbook/utility-types.html)
