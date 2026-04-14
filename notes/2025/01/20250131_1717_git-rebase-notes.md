# Git rebase reference

Quick reference for interactive rebase operations I forget often enough to warrant writing down.

## Basic interactive rebase

```
git rebase -i HEAD~N
```

Opens an editor with the last N commits. Each line starts with a command word followed by the commit hash and message.

## Useful commands in the rebase editor

- `pick` — use commit as-is
- `reword` — use commit but edit the message
- `edit` — pause after applying commit for amending
- `squash` — meld into previous commit, combining messages
- `fixup` — meld into previous commit, discarding this message
- `drop` — remove the commit entirely

## Common use cases

**Squash last 3 commits into one:**
Change the second and third `pick` to `squash`, then write a combined message when prompted.

**Reorder commits:**
Simply reorder the lines in the editor. Git will replay them in the new order.

**Fix an old commit message:**
Use `reword` on the target commit.

## Recovering from a bad rebase

```
git reflog
git reset --hard HEAD@{N}
```

The reflog is your friend. It tracks every position the HEAD has been at, even after destructive operations.

More detail at the official docs: [https://git-scm.com/docs/git-rebase](https://git-scm.com/docs/git-rebase)

Also worth reading: the section on rerere (Reuse Recorded Resolution) if you rebase branches that frequently diverge from the same base.
