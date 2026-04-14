# TypeScript utility types reference

These are the ones I reach for regularly. The full list is in the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/utility-types.html).

## Partial and Required

```typescript
type Config = { host: string; port: number; debug: boolean };

type PartialConfig = Partial<Config>;    // all optional
type FullConfig = Required<PartialConfig>; // all required again
```

## Pick and Omit

```typescript
type UserPreview = Pick<User, 'id' | 'name' | 'avatarUrl'>;
type UserWithoutPassword = Omit<User, 'passwordHash'>;
```

## Record

```typescript
type StatusMap = Record<string, 'ok' | 'error' | 'pending'>;
const statuses: StatusMap = { users: 'ok', payments: 'error' };
```

## Extract and Exclude

```typescript
type Events = 'click' | 'focus' | 'blur' | 'submit';
type FocusEvents = Extract<Events, 'focus' | 'blur'>; // 'focus' | 'blur'
type NonFocus = Exclude<Events, 'focus' | 'blur'>;   // 'click' | 'submit'
```

## ReturnType and Parameters

```typescript
function getUser(id: number): Promise<User> { ... }

type UserResult = ReturnType<typeof getUser>; // Promise<User>
type GetUserParams = Parameters<typeof getUser>; // [id: number]
```

## Awaited

Unwraps a Promise type recursively:

```typescript
type User = Awaited<ReturnType<typeof getUser>>; // User
```

## Template literal types

```typescript
type EventName = 'click' | 'focus';
type HandlerName = `on${Capitalize<EventName>}`; // 'onClick' | 'onFocus'
```

Useful for generating typed property names from string unions.

