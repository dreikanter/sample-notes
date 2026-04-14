---
title: Reading notes — Designing data-intensive applications
slug: book-notes-kleppmann
tags: [books, databases, distributed-systems]
public: true
---

# Reading notes — Designing data-intensive applications

Finally started this properly rather than reading chapters out of order as reference. Working through it front to back. About 200 pages in (out of ~550 with appendices).

## Part I: Foundations of Data Systems

**Chapter 1** sets up the framework well: reliability, scalability, maintainability. The distinction between latency and response time (latency is a duration, response time is what the client observes including queuing, processing, etc.) is the kind of precision that makes the whole book worth reading.

**Chapter 2** on data models is excellent. The relational vs document vs graph model discussion is clearer than anything else I've read on this. Key takeaway: the relational model handles many-to-many relationships better than document models; document models handle tree-structured data with fewer joins. The question is which structure matches your access patterns.

**Chapter 3** on storage engines: B-trees vs LSM-trees. B-trees update in place; LSM-trees write sequentially (faster writes) and compact later. SSDs have changed the tradeoffs somewhat but the fundamentals still hold. The write amplification concept is useful.

## What I'm taking away so far

The emphasis on understanding tradeoffs rather than picking "the best database" is refreshing. Each choice has costs; the goal is to make them explicit.

Companion resources: [https://dataintensive.net](https://dataintensive.net)
