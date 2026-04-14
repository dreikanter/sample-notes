# Git rebase workflow notes

I keep having to look this up, so writing it down properly.

## The case for rebase

Merge commits create noise in long-running repos. Rebase rewrites your branch on top of the target, giving a clean linear history. The tradeoff: you rewrite commit SHAs, so never rebase shared branches.

## Interactive rebase

```bash
git rebase -i HEAD~5   # edit last 5 commits
```

In the editor:
- `pick` — keep commit as-is
- `reword` — keep but edit message
- `squash` — merge into previous, combine messages
- `fixup` — merge into previous, discard message
- `drop` — remove the commit entirely
- `edit` — pause here to amend the commit

## Rebasing onto main

```bash
git fetch origin
git rebase origin/main
```

If conflicts arise, resolve them, then:

```bash
git add <resolved files>
git rebase --continue
```

To abort and return to the pre-rebase state:

```bash
git rebase --abort
```

## Autosquash

If your commit message starts with `fixup!` or `squash!` followed by the target commit message, `--autosquash` will arrange and mark them automatically:

```bash
git commit -m "fixup! Add user login"
git rebase -i --autosquash origin/main
```

## The reflog safety net

If you mess something up, `git reflog` shows every HEAD movement. Find the SHA before the rebase and:

```bash
git reset --hard <that-SHA>
```

Full documentation: [git-rebase man page](https://git-scm.com/docs/git-rebase)
