# Rust ownership — working notes

Coming from Python/TypeScript, the ownership model takes real mental adjustment. These are the points where my intuition was wrong.

## Move semantics are the default

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is MOVED, not copied
// println!("{}", s1);  // Error: value borrowed after move
```

For `Copy` types (integers, bools, chars, tuples of Copy types), assignment copies rather than moves. For heap-allocated types, you move.

## Borrowing rules

```rust
let mut s = String::from("hello");

// Multiple immutable borrows: OK
let r1 = &s;
let r2 = &s;
println!("{} {}", r1, r2);

// One mutable borrow: OK (but no other borrows active)
let r3 = &mut s;
r3.push_str(" world");
```

The compiler enforces that mutable and immutable borrows don't coexist. This prevents data races at compile time.

## Lifetimes appear when references outlive data

```rust
// Error: returned reference would outlive local data
fn bad() -> &str {
    let s = String::from("hello");
    &s  // s dropped here, reference dangling
}

// OK: reference lives as long as input
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

## The pattern that helped it click

Think of ownership as a guarantee that exactly one thing is responsible for cleanup. Moves transfer that responsibility. Borrows are temporary access without responsibility transfer. The borrow checker ensures borrows don't outlive the owner.

Rust book: https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html
