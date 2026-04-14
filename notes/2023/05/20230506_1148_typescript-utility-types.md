# TypeScript utility types — reference

Built-in utility types I use regularly. Reference to save looking them up.

**`Partial<T>`** — makes all properties optional
```typescript
type User = { id: number; name: string; email: string };
type PartialUser = Partial<User>; // all optional
```

**`Required<T>`** — makes all properties required (inverse of Partial)

**`Readonly<T>`** — makes all properties read-only

**`Pick<T, K>`** — subset of properties
```typescript
type UserPreview = Pick<User, 'id' | 'name'>;
```

**`Omit<T, K>`** — all properties except K
```typescript
type NewUser = Omit<User, 'id'>; // for creation, before ID is assigned
```

**`Record<K, V>`** — object type with keys K and values V
```typescript
type Counters = Record<string, number>;
```

**`ReturnType<T>`** — extract return type of a function
```typescript
type Result = ReturnType<typeof myFunction>;
```

**`Parameters<T>`** — extract parameter types as tuple
```typescript
type Params = Parameters<typeof fetch>; // [input: RequestInfo, init?: RequestInit]
```

**`NonNullable<T>`** — removes null and undefined
```typescript
type StringOnly = NonNullable<string | null | undefined>; // string
```

**`Extract<T, U>` / `Exclude<T, U>`**
```typescript
type OnlyStrings = Extract<string | number | boolean, string>; // string
type NoStrings = Exclude<string | number | boolean, string>; // number | boolean
```

Full reference: [TypeScript handbook utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
