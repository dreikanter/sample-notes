# Git rebase workflow

Notes on interactive rebase — I keep getting the sequence wrong.

## Basic interactive rebase

```bash
git rebase -i HEAD~5      # edit last 5 commits
git rebase -i main        # rebase branch from where it diverged from main
```

## Commands in the editor

```
pick   — use commit as-is
reword — use commit, but edit message
edit   — use commit, stop to amend content
squash — meld into previous commit, combine messages
fixup  — meld into previous commit, discard this message
drop   — remove commit entirely
```

## Common patterns

**Squash a WIP commit into the previous one:**
Change the WIP commit's `pick` to `fixup`. It disappears into the prior commit.

**Reorder commits:**
Move lines in the editor. The topmost line is oldest (applied first).

**Split a commit:**
Mark it `edit`. When git stops, do `git reset HEAD^`, stage selectively, create separate commits, then `git rebase --continue`.

## Conflict resolution

When a conflict occurs during rebase:
```bash
# Fix conflicts in the files
git add <resolved-files>
git rebase --continue    # move to next commit

# If you want to abort
git rebase --abort       # returns to original state
```

## Gotcha: never rebase shared branches

Interactive rebase rewrites history. If you rebase commits that exist on origin, you create divergence. Only safe on local or feature branches that haven't been pushed, or after coordinating with the team.

## autosquash

If you commit with `git commit --fixup=<sha>`, then `git rebase -i --autosquash` automatically arranges fixup commits — eliminates manual editing.

Reference: https://git-scm.com/docs/git-rebase
