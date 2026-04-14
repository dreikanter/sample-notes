---
title: TypeScript utility types I actually use
slug: typescript-utility-types
tags: [typescript, programming, reference]
---

# TypeScript utility types I actually use

The [TypeScript handbook on utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) lists all of them, but here are the ones I reach for regularly with concrete examples.

## Partial and Required

```typescript
type User = { name: string; email: string; bio: string; };

// All optional — useful for update payloads
type UserUpdate = Partial<User>;

// All required — useful when you know all fields are present
type FullUser = Required<User>;
```

## Pick and Omit

```typescript
// Just what you need
type UserPreview = Pick<User, 'name' | 'email'>;

// Everything except what you don't want
type PublicUser = Omit<User, 'email'>;
```

## ReturnType and Parameters

```typescript
function fetchUser(id: number, options?: { cache: boolean }) {
  return { id, name: 'Alex' };
}

type FetchUserReturn = ReturnType<typeof fetchUser>;
// { id: number; name: string; }

type FetchUserParams = Parameters<typeof fetchUser>;
// [id: number, options?: { cache: boolean } | undefined]
```

## Record

```typescript
type Status = 'active' | 'inactive' | 'pending';
type StatusConfig = Record<Status, { label: string; color: string }>;
```

## NonNullable

```typescript
type MaybeUser = User | null | undefined;
type DefiniteUser = NonNullable<MaybeUser>; // User
```

## Awaited

```typescript
type UserPromise = Promise<User>;
type ResolvedUser = Awaited<UserPromise>; // User
```

The ones I almost never reach for in practice: `Readonly`, `Extract`, `Exclude`. They exist for good reasons but I don't encounter the patterns often in application code.
