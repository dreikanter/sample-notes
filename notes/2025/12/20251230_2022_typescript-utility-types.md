# TypeScript utility types

Quick reference for built-in utility types I always look up.

## Transformation types

```typescript
Partial<T>          // all properties optional
Required<T>         // all properties required
Readonly<T>         // all properties readonly
Record<K, V>        // object with keys K and values V
```

## Filtering types

```typescript
Pick<T, K>          // keep only keys K from T
Omit<T, K>          // remove keys K from T
Exclude<T, U>       // from T, remove what's assignable to U
Extract<T, U>       // from T, keep only what's assignable to U
NonNullable<T>      // remove null and undefined
```

## Function types

```typescript
Parameters<T>       // tuple of function parameter types
ReturnType<T>       // return type of a function
ConstructorParameters<T>  // constructor parameter types
InstanceType<T>     // instance type of a constructor
```

## Practical combos

Remove a property and make result partial:
```typescript
type WithoutId<T> = Partial<Omit<T, 'id'>>
```

Make specific keys optional:
```typescript
type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>
```

## Template literal types (4.1+)

```typescript
type EventName = `on${Capitalize<string>}`
```

Full reference: https://www.typescriptlang.org/docs/handbook/utility-types.html

Also worth reading: https://type-challenges.github.io — working through these challenges is the fastest way to internalize this stuff.
