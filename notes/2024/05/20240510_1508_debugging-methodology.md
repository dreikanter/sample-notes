---
title: Debugging methodology
slug: debugging-methodology
tags: [programming, debugging, process, software]
description: A systematic approach to debugging that I keep having to re-derive
---

# Debugging methodology

Every senior developer seems to debug efficiently without being able to explain how. This is my attempt to make the implicit explicit for myself.

## The core loop

1. Reproduce reliably
2. Form a hypothesis
3. Test the hypothesis (don't fix yet)
4. Narrow or eliminate
5. Fix
6. Verify the fix in the way that would have caught this

## Step 1 is often skipped and shouldn't be

If you can't reproduce the bug reliably, you can't tell whether your fix worked. Before doing anything else: find the minimal conditions that produce the bug. This often reveals the cause.

Intermittent bugs: log everything you can think of, then wait. The logs around the occurrence usually tell you.

## The hypothesis trap

People jump to fixing before forming a hypothesis about the cause. The fix might work but you don't learn anything and you might introduce new problems. 

A hypothesis is: "I believe X is happening because Y." If X, then I should see Z. Check for Z.

## Bisection

When you don't know when a regression was introduced, binary search through the history. Half the range, test, eliminate half, repeat. Git bisect automates this.

## The rubber duck technique works

Explaining the problem in detail to another person (or an inanimate object) forces the articulation that often reveals the answer. The act of explaining requires organizing information in a way that internal thought doesn't.

## When stuck

- Change the medium: draw a diagram, write out the state
- Walk away (for bugs that take more than an hour: time away is often the variable)
- Explain it to someone else
- Question your assumptions — state them explicitly and ask if each one is actually known

## After the fix

Write a test that would have caught it. Understand why the existing tests didn't catch it. Consider whether the same pattern exists elsewhere in the codebase.

https://jvns.ca/blog/2019/06/23/a-few-ways-to-make-debugging-harder/
