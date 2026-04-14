# Rust ownership mental model

Learning Rust on and off. The ownership system is the hardest part and I keep needing to rebuild my mental model. Writing it out helps.

## The three rules

1. Each value in Rust has a single owner.
2. When the owner goes out of scope, the value is dropped.
3. There can be either one mutable reference OR any number of immutable references at a time — not both.

## Move vs Copy

When you assign a heap-allocated value (String, Vec, Box), ownership moves:

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is moved, no longer valid
// println!("{}", s1);  // compile error
```

Types that implement `Copy` (integers, booleans, f64, tuples of Copy types) are copied instead:

```rust
let x = 5;
let y = x;  // x is still valid, it was copied
```

## Borrowing

References let you refer to a value without taking ownership:

```rust
fn print_length(s: &String) {
    println!("{}", s.len());
}

let s = String::from("hello");
print_length(&s);  // pass a reference
println!("{}", s);  // s still valid
```

The rule: one mutable OR many immutable — this is what prevents data races at compile time.

## Lifetime annotations

When do you need them? When the compiler can't figure out how long a reference lives relative to what it refers to. In practice: mostly in struct definitions and function signatures with multiple reference parameters.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Still finding this the least intuitive part.

[The Rust Book — Understanding Ownership](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)
