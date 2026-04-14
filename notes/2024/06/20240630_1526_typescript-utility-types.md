# TypeScript utility types cheatsheet

Reference for the utility types I keep forgetting or looking up.

## Partial and Required

```typescript
type Partial<T> = { [P in keyof T]?: T[P] };
type Required<T> = { [P in keyof T]-?: T[P] };
```

`Partial<User>` makes every field optional. `Required` does the reverse — useful for removing optionality added by `Partial`.

## Pick and Omit

```typescript
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

Pick to select only certain fields; Omit to exclude them. I use Omit more often in practice.

## Extract and Exclude (for union types)

```typescript
type Extract<T, U> = T extends U ? T : never;
type Exclude<T, U> = T extends U ? never : T;
```

Example:
```typescript
type Status = "pending" | "active" | "archived";
type ActiveStatus = Extract<Status, "active" | "pending">;  // "active" | "pending"
type NotPending = Exclude<Status, "pending">;  // "active" | "archived"
```

## ReturnType and Parameters

```typescript
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
```

Extremely useful for typing functions whose return type is inferred from a third-party library.

## NonNullable

Removes `null` and `undefined` from a union.

```typescript
type T = NonNullable<string | null | undefined>;  // string
```

Full reference: [TypeScript Handbook — Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
