# Rust ownership — first principles

Started Chapter 4 of The Rust Book today. The ownership system is the thing that makes Rust Rust, and it's different enough from every other language I know that I had to stop and think slowly.

## The three rules

1. Each value in Rust has a single owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value is dropped.

These rules exist so the compiler can manage memory without a garbage collector. No GC pause, no use-after-free. The cost is that you think about ownership at write time rather than at runtime.

## Move semantics

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is now invalid. s1 has been "moved" to s2.
println!("{}", s1);  // compile error: borrow of moved value
```

This is different from shallow copy (which would leave both s1 and s2 pointing at the same heap data, risking double-free) and from deep copy (which is explicit via `.clone()`).

## Borrowing

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}  // s goes out of scope but doesn't drop the string, because it doesn't own it
```

References allow you to refer to a value without taking ownership. The function borrows the string, uses it, and returns the borrow.

## Why this matters

In systems where memory allocation failures are catastrophic (embedded, real-time, security-critical), knowing statically that you have no dangling pointers or data races is worth significant upfront cognitive cost.

Resource: [The Rust Programming Language, Chapter 4](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html). Free, accurate, surprisingly readable.
