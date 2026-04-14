---
title: Git rebase workflow for feature branches
slug: git-rebase-workflow
tags: [git, workflow, tools]
description: Notes on keeping feature branches clean with interactive rebase.
---

# Git rebase workflow for feature branches

The pattern I've settled on after a few painful experiences with messy merge histories.

## The core loop

```bash
# Keep your branch current
git fetch origin
git rebase origin/main

# Before PR: clean up commits
git rebase -i origin/main
```

Interactive rebase lets you squash fixup commits, reword messages, and reorder where it makes sense. The [Git book section on rewriting history](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) is the best reference.

## Rules I follow

1. Never rebase a branch someone else is working from. If in doubt, merge instead.
2. Squash "fix typo" and "WIP" commits before the PR — they add noise to `git log`.
3. Keep commits logically atomic: each commit should be independently understandable and (ideally) deployable.
4. Write commit messages in imperative mood: "Add user authentication" not "Added user authentication".

## Handling conflicts

Rebase conflicts are identical to merge conflicts but they appear one commit at a time, which is actually easier to reason about. For each conflict:

```bash
# edit the conflicted file, then:
git add <file>
git rebase --continue
```

If things get tangled: `git rebase --abort` returns you to where you started.

## When to use merge instead

- Long-running feature branches where the history of the integration matters
- Shared branches
- When you want to preserve the exact topology of when work happened

Force-pushing after a rebase requires `-f` or `--force-with-lease`. Always prefer `--force-with-lease` — it refuses if someone else pushed in the meantime.
