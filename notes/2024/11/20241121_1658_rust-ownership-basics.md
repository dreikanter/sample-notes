# Rust ownership — working through the basics

Working through the [Rust book](https://doc.rust-lang.org/book/) and making notes on the ownership system, which is the part that requires the most mental adjustment coming from garbage-collected languages.

## The three rules

1. Each value has a single owner
2. Only one owner at a time
3. When the owner goes out of scope, the value is dropped

## Move semantics

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is moved to s2 — s1 is now invalid
println!("{}", s1);  // Compile error: s1 has been moved
```

Contrast with integers, which implement `Copy` and are duplicated instead of moved:
```rust
let x = 5;
let y = x;
println!("{} {}", x, y);  // Fine — integers are Copy
```

## Borrowing with references

```rust
let s = String::from("hello");
let len = calculate_length(&s);  // Pass a reference
println!("{}", s);  // s is still valid
```

Rules for references:
- At any time: **either** one mutable reference **or** any number of immutable references — but not both
- References must always be valid (no dangling references)

## Why the mutable reference restriction?

Prevents data races at compile time. Two threads holding mutable references to the same data is undefined behaviour in C; in Rust it won't compile.

```rust
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s;  // Compile error: cannot borrow `s` as mutable more than once
```

## Slices

Slices are references to a contiguous sequence of elements. They don't own their data:
```rust
let s = String::from("hello world");
let word = &s[0..5];  // "hello" — a string slice
```

The connection I keep coming back to: once you accept that the borrow checker is preventing entire classes of runtime errors (use-after-free, double-free, data races), the friction starts feeling like protection rather than obstruction. Mostly.
