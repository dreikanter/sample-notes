---
title: Git rebase cheatsheet
slug: git-rebase-cheatsheet
tags: [git, dev, cheatsheet]
description: Commands and mental models for interactive rebase and branch management
public: true
---

# Git rebase cheatsheet

A working reference for operations I look up too often.

## Interactive rebase

```bash
git rebase -i HEAD~N          # rebase last N commits
git rebase -i <base-commit>   # rebase from a specific commit
```

In the editor, actions per commit:
- `pick` — keep as-is
- `reword` — change message only
- `edit` — pause to amend
- `squash` — merge into previous commit, combine messages
- `fixup` — merge into previous, discard this message
- `drop` — remove entirely

## Rebase onto a different base

```bash
git rebase main              # rebase current branch onto main
git rebase --onto new-base old-base branch
```

The `--onto` form is useful when you branched off a feature branch and want to rebase just your commits onto main, leaving the feature branch's commits behind.

## Conflict resolution

```bash
git rebase --continue        # after resolving conflicts
git rebase --abort           # bail out, restore original state
git rebase --skip            # skip the conflicting commit
```

## Golden rule

Never rebase commits that have been pushed to a shared remote. The SHA changes; anyone who based work on those commits will have a divergent history. Local-only branches are fair game.

Full documentation: https://git-scm.com/docs/git-rebase

Good mental model from Julia Evans: https://jvns.ca/blog/2023/11/06/rebasing-what-can-go-wrong-/
