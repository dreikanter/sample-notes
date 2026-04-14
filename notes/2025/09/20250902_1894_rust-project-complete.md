---
title: Rust file watcher — project complete
slug: rust-project-complete
tags: [rust, programming, projects]
description: Notes on completing the first real Rust project
public: true
---

# Rust file watcher — project complete

The file watcher project [[20250819_1886]] is done. It's a small CLI tool that watches a directory and reports file changes with timestamps and event types (created, modified, deleted). About 300 lines of Rust.

**What it does**

```
$ fw watch ./project --ignore "*.tmp" --ignore ".git"
[14:32:01] MODIFIED  src/main.rs
[14:32:03] CREATED   src/helpers.rs
[14:32:07] DELETED   src/old.rs
```

Uses the `notify` crate for cross-platform file system events. Supports glob patterns for ignoring files. Output to stdout by default, can write to a log file with `--output`.

**What building it taught me**

The ownership model clicked properly around week two. The moment it stopped feeling like fighting the compiler and started feeling like the compiler catching real bugs I was about to make — that was the turning point.

Pattern matching in Rust is excellent. `match` expressions on event types, combined with enum variants, produce readable code without long if-else chains.

Error handling with `Result<T, E>` and the `?` operator is nicer than exception handling in Python once you understand it. Errors propagate explicitly; you always know what can fail.

**What I still don't understand well**

Lifetimes in complex cases. My code uses simple lifetime annotations and mostly lets the compiler infer. The cases where you need to specify are still mysterious to me.

**Next Rust project**

Probably a small HTTP server using Axum, to learn the async Rust ecosystem properly.

The Rust notify crate: https://docs.rs/notify/latest/notify/
