# TypeScript utility types — working reference

The built-in utility types I reach for most often and the cases where they don't work how you'd expect.

## Partial, Required, Readonly

```typescript
type Partial<T>   // all properties optional
type Required<T>  // all properties required
type Readonly<T>  // all properties readonly
```

`Partial` is useful for update functions: `function update(id: string, changes: Partial<User>)`. The problem is it makes *everything* optional, including properties that logically shouldn't be. `Pick` combined with `Partial` is usually more precise.

## Pick and Omit

```typescript
type UserPreview = Pick<User, 'id' | 'name' | 'email'>;
type UserWithoutPassword = Omit<User, 'passwordHash'>;
```

`Omit` is frequently better for API responses — safer to explicitly exclude sensitive fields than to remember to include every safe field.

## Record

```typescript
type StatusMap = Record<OrderStatus, string>;
// OrderStatus = 'pending' | 'shipped' | 'delivered'
```

Forces you to handle every member of a union. Catches the "added a new status but forgot to update the UI mapping" class of bugs.

## ReturnType and Parameters

```typescript
type HandlerReturn = ReturnType<typeof handleRequest>;
type HandlerArgs = Parameters<typeof handleRequest>;
```

Most useful when working with functions you don't control. You can extract types without importing them separately.

## Conditional types

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;
```

The `extends ... ? ... : ...` pattern is powerful and becomes necessary for generic utilities. Understanding `infer` is the next step — it's used inside conditional types to capture and re-use matched types.

Full reference: [TypeScript handbook on utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html).
