# Rust ownership – early notes

Three weeks into Rust via the official book (https://doc.rust-lang.org/book/) and Exercism exercises. The ownership system is the thing everyone warns you about, and the warnings are accurate.

## The three rules

1. Each value has one owner.
2. There can be only one owner at a time.
3. When the owner goes out of scope, the value is dropped.

Simple to state. The implications cascade.

## What trips me up

**Move semantics**: when you assign a non-Copy type to another variable, the original is no longer valid.

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is moved into s2
// println!("{}", s1);  // compile error: s1 moved
```

This is counterintuitive coming from any GC language. The compiler treats it as a bug, which it kind of is — you're trying to use something that was given away.

**Borrowing**: passing references instead of moving. `&T` for shared (immutable) borrows, `&mut T` for exclusive (mutable) borrows. The rule: you can have either many shared borrows OR one mutable borrow, never both simultaneously.

This prevents data races at compile time. It's elegant in retrospect but infuriating when you're fighting it.

**Lifetimes**: haven't fully grasped these yet. The compiler often infers them; I'll return to this.

## What's clicking

Once you accept the constraint, you stop reaching for global mutable state as a solution. The borrow checker is forcing better design choices.

Exercism profile: https://exercism.org/profiles/me
