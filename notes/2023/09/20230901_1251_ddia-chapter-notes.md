---
title: DDIA chapter 3 — storage engines notes
slug: ddia-chapter-notes
tags: [reading, databases, engineering]
description: Notes on the storage and retrieval chapter of Designing Data-Intensive Applications.
---

# DDIA chapter 3 — storage engines notes

Chapter 3 of Kleppmann's book covers storage engines — specifically the divide between log-structured (LSM-tree based) and page-oriented (B-tree based) storage.

Full book: https://dataintensive.net/

## Log-structured merge-trees (LSM trees)

Writes go to an in-memory structure (memtable), which is periodically flushed to disk as a sorted string table (SSTable). Reads check the memtable first, then SSTables in order from newest to oldest, with Bloom filters to skip tables that definitely don't contain the key.

Key insight: sequential writes to disk are orders of magnitude faster than random writes, especially on spinning disk. LSM-trees turn all writes into sequential appends.

Used in: LevelDB, RocksDB, Cassandra, HBase.

**Tradeoffs:**
- Write amplification: a key may be written multiple times during compaction
- Read amplification: in the worst case, checking multiple SSTables
- Good for write-heavy workloads, especially time-series

## B-trees

The dominant structure in traditional relational databases (Postgres, MySQL, SQLite). Data stored in fixed-size pages (typically 4–8KB), organized as a balanced tree. Read: traverse from root to leaf page. Write: find the leaf page, write in place, update parent if page splits.

**Tradeoffs:**
- Write-in-place means random I/O
- Well-suited for read-heavy workloads and point lookups
- Predictable performance characteristics — depth of tree is bounded by O(log n)

## Key takeaway

Neither is universally better. LSM-trees are often faster for writes and B-trees are often faster for reads, but the actual answer depends on your workload. Profile before deciding.
