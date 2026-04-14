# Git bisect workflow

Used `git bisect` properly for the first time today instead of just guessing which commit broke things. Documenting the workflow.

## Basic usage

```bash
git bisect start
git bisect bad                    # current commit is broken
git bisect good v2.1.0            # last known good state

# git checks out a middle commit
# test manually or run a script
# then mark it:
git bisect good
# or:
git bisect bad

# repeat until git identifies the culprit
# when done:
git bisect reset
```

## Automating with a test script

This is the actually useful part. If you have a test that fails on the broken state:

```bash
git bisect start
git bisect bad HEAD
git bisect good abc123
git bisect run ./test-for-regression.sh
```

The script must exit 0 for good commits and non-zero for bad ones. Git will automatically bisect through the history and report the first bad commit.

## What I found today

Regression in our cache invalidation was introduced in commit `e4f8a92` on June 3rd. The commit message was "minor cleanup" — not minor. Found it in about 4 minutes instead of the 45 I'd spent guessing before.

Logarithmic search is obviously the right tool here. O(log n) across commit history.

Full reference: [Git bisect documentation](https://git-scm.com/docs/git-bisect)
