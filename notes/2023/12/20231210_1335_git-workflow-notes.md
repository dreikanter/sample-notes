# Git workflow notes — team conventions

Documenting our current conventions after the third incident of someone rebasing a shared branch.

## Branch naming

```
feature/SHORT-123-brief-description
fix/SHORT-456-what-was-broken
chore/SHORT-789-what-maintenance
```

Lowercase, hyphens, ticket number. The ticket number is non-negotiable since Jira auto-linking depends on it.

## Commit messages

We follow Conventional Commits loosely:

```
feat: add retry logic to background job processor
fix: prevent null pointer in token refresh edge case
chore: update postgres client to 15.x
docs: document event pipeline architecture
```

No period at the end. Imperative mood. Under 72 characters. Body is optional but encouraged for anything non-obvious.

## The rebase rule

Do not rebase branches that others have checked out. If you need to clean up commits, do it before opening the PR or use `git commit --fixup` + `git rebase -i --autosquash` locally before pushing.

Once a PR is open, use `git merge main` to stay current. Squash on merge via GitHub so main stays clean.

## Useful commands I keep forgetting

```bash
git log --oneline --graph --all   # visual branch map
git stash push -m "description"   # named stash
git bisect start && git bisect bad && git bisect good <SHA>  # binary search for regressions
git diff main...HEAD              # changes since branch diverged
```

Reference: [Conventional Commits specification](https://www.conventionalcommits.org/) and [Atlassian's Git tutorials](https://www.atlassian.com/git/tutorials) for the workflow foundations.
