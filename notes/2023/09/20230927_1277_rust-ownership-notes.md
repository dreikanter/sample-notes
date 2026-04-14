---
title: Rust ownership — mental model notes
slug: rust-ownership-notes
tags: [rust, programming, learning]
description: Working through Rust ownership concepts — notes for my own reference.
---

# Rust ownership — mental model notes

Starting to learn Rust seriously. The borrow checker is the hard part for everyone; writing this to crystallize the model.

Official reference: https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html

## The three rules

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value is dropped.

## Move semantics

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is moved into s2
// s1 is no longer valid here
println!("{}", s1);  // compile error
```

This is different from most languages. The String is not copied — ownership transfers. The old variable becomes invalid. This prevents double-free errors.

For types that implement `Copy` (integers, booleans, f64, etc.), assignment copies the value and both variables remain valid.

## Borrowing

```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);  // borrow, not move
println!("{}, {}", s1, len);  // s1 still valid
```

The `&` creates a reference. A reference lets you refer to a value without taking ownership.

## Mutable references

```rust
let mut s = String::from("hello");
let r1 = &mut s;
// let r2 = &mut s;  // compile error: can't have two mutable references
```

You can have exactly one mutable reference to a value at a time. This prevents data races at compile time — a genuinely useful guarantee.

## The common confusion

You can have either:
- Any number of immutable references, OR
- Exactly one mutable reference

Not both at the same time. The compiler enforces this. It takes adjustment but then becomes an asset.
