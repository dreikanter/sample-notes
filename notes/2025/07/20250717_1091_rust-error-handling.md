---
title: Rust error handling patterns
slug: rust-error-handling
tags: [rust, programming, error-handling]
description: thiserror vs anyhow and when to use each
---

# Rust error handling patterns

Working through a small CLI project and kept second-guessing the error handling approach. This is my working summary after reading through several library codebases.

## The core choice

**`anyhow`** — for applications (binaries). Wraps any error into a single opaque type with `.context()` for adding messages. Fast to write, terrible for downstream error matching.

**`thiserror`** — for libraries. Derives `std::error::Error` on your own enum. Structured, matchable, but more boilerplate.

```rust
// thiserror
#[derive(Debug, thiserror::Error)]
enum ParseError {
    #[error("invalid header: {0}")]
    InvalidHeader(String),
    #[error("unexpected EOF at byte {pos}")]
    UnexpectedEof { pos: usize },
}

// anyhow
fn read_config(path: &Path) -> anyhow::Result<Config> {
    let text = std::fs::read_to_string(path)
        .with_context(|| format!("reading {}", path.display()))?;
    // ...
}
```

## When to combine them

Application code can use `anyhow` at the top level while calling library functions that return `thiserror` enums. The `?` operator converts automatically via `From`.

## The `?` operator chain

The `?` after a `Result` expression is equivalent to:

```rust
match expr {
    Ok(val) => val,
    Err(e) => return Err(e.into()),
}
```

The `.into()` is where `From` conversions happen, so your error enum needs `From` implementations for all source types — `thiserror` handles this with `#[from]`.

Official error handling guide: https://doc.rust-lang.org/book/ch09-00-error-handling.html

Related: [[20251018_1015]] covers Rust ownership which is a prerequisite for understanding why error types need to be `Send + Sync` in async contexts.
