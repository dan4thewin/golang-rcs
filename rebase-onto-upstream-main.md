# Rebase onto upstream main

**Source:** ProjectsToDo (Apr 2026)
**Status:** Required before PR submission

Upstream is very active (621 commits since Jan 2025). The fork point is afc454e (Mar 28). Rebase `add-ci` onto current `upstream/main` before opening any PR.

Run:
```bash
git fetch upstream
git rebase upstream/main
```

Resolve conflicts (likely in `co.go` due to the diff/znkr changes).