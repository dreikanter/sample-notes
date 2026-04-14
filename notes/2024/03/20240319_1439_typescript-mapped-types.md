# TypeScript mapped types

A mapped type creates a new type by iterating over the keys of another type. Essential for transformation utilities.

## Basic pattern

```typescript
type Optional<T> = {
  [K in keyof T]?: T[K];
};

// Equivalent to Partial<T>
```

## Modifiers

```typescript
// Make all properties required and readonly
type Strict<T> = {
  readonly [K in keyof T]-?: T[K];
  //                    ^ remove optional modifier
};

// Remove readonly
type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};
```

## Key remapping (4.1+)

```typescript
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

interface Person { name: string; age: number; }
type PersonGetters = Getters<Person>;
// { getName: () => string; getAge: () => number; }
```

## Filtering keys

```typescript
// Keep only string-valued properties
type StringProps<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K];
};
```

## Practical example: form state

```typescript
type FormState<T> = {
  values: T;
  errors: { [K in keyof T]?: string };
  touched: { [K in keyof T]?: boolean };
};
```

## Template literal types with mapped

```typescript
type EventHandlers<T> = {
  [K in keyof T as `on${Capitalize<string & K>}Change`]?: (val: T[K]) => void;
};
```

These patterns cover most of what the built-in utility types (Partial, Required, Readonly, Pick, Omit) don't directly address.

Docs: https://www.typescriptlang.org/docs/handbook/2/mapped-types.html
