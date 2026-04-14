---
title: Go error handling patterns
slug: go-error-handling
tags: [go, dev, patterns]
description: Practical patterns for Go error handling and wrapping
---

# Go error handling patterns

## Basic pattern

```go
result, err := doSomething()
if err != nil {
    return fmt.Errorf("doSomething: %w", err)
}
```

The `%w` verb wraps the error, preserving it for `errors.Is` and `errors.As` checks higher up.

## Wrapping with context

Always add context when returning errors:
```go
// Bad — caller doesn't know what failed
return err

// Good — wraps with context
return fmt.Errorf("loading config from %s: %w", path, err)
```

The convention: `component/operation: original error`. This creates readable error chains.

## Checking wrapped errors

```go
// Check error identity
if errors.Is(err, fs.ErrNotExist) {
    // handle file-not-found
}

// Extract error type
var pathErr *fs.PathError
if errors.As(err, &pathErr) {
    fmt.Println("path:", pathErr.Path)
}
```

Both `Is` and `As` unwrap the chain automatically.

## Sentinel errors

```go
var (
    ErrNotFound = errors.New("not found")
    ErrConflict = errors.New("conflict")
)

func GetUser(id int) (*User, error) {
    if user == nil {
        return nil, fmt.Errorf("user %d: %w", id, ErrNotFound)
    }
    return user, nil
}

// Caller:
if errors.Is(err, ErrNotFound) { /* handle */ }
```

## Custom error types

```go
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error on %s: %s", e.Field, e.Message)
}
```

## Avoid: swallowing errors

```go
// Never do this unless truly intentional
result, _ := doSomething()

// If intentional, comment why
_ = closeFile() // error unrecoverable; best effort
```

Reference: https://go.dev/blog/go1.13-errors
Effective error messages: https://github.com/golang/go/wiki/ErrorValueFAQ
