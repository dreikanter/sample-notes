---
title: Distributed systems — Raft consensus notes
slug: distributed-systems-raft-notes
tags: [distributed-systems, raft, consensus, learning]
description: Notes from studying the Raft consensus protocol
public: true
---

# Distributed systems — Raft consensus notes

Working through the [Raft paper](https://raft.github.io/raft.pdf) by Ongaro and Ousterhout alongside the MIT 6.824 lectures.

## The core problem Raft solves

Multiple servers need to agree on a sequence of log entries even when some servers fail or are temporarily unavailable. Raft is a consensus algorithm for this.

## Three sub-problems Raft separates

1. **Leader election** — one server acts as leader; if it fails, a new one is elected
2. **Log replication** — leader accepts client requests and replicates them to followers
3. **Safety** — once an entry is committed, it must appear in future leaders' logs

## Leader election

Leaders send periodic heartbeats. If a follower doesn't receive one within a random election timeout (150-300ms by default), it becomes a candidate and requests votes. The randomization prevents simultaneous candidacies from splitting the vote indefinitely.

A candidate wins if it receives votes from a majority. Majority voting prevents split-brain — at most one leader can be elected in any term.

## The part I found confusing

Log commitment requires careful thought. A leader can't commit an entry from a *previous* term by counting replicas — even if an old entry is replicated to a majority, it might not be committed if the leader changes. A new leader must commit an entry from the *current* term first, which implicitly commits all earlier entries.

The paper's Figure 8 illustrates the problematic case. Worth drawing out.

## Current understanding confidence

- Leader election: high
- Log replication basics: medium
- Log commitment edge cases: low — need to re-read section 5.4.2

Visualisation tool: [Raft scope](https://raft.github.io/) — interactive and genuinely helpful.
