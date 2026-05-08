# Split co.go bugfix into separate PR

**Severity:** High (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

## Problem

The `applyDelta` empty-output fix and `trailingNewline` removal in
`co.go` are logically independent of the `ci` feature. Bundling them
makes review harder and raises the chance of unrelated discussion
blocking the feature PR.

## What changed in co.go

```go
// Before (buggy):
return strings.Join(w.lines, "\n") + trailingNewline(from), nil

// After (fixed):
if len(w.lines) == 0 {
    return "", nil
}
return strings.Join(w.lines, "\n") + "\n", nil
```

Plus removal of the dead `trailingNewline()` helper function.

## How to split

```bash
cd ~/src/golang-rcs-project/code
git checkout main
git checkout -b fix-applydelta-empty

# Cherry-pick or recreate just the co.go changes
# The relevant diff is in the add-ci branch, commit eaa83ff or later

git push origin fix-applydelta-empty
# Open PR #1 from fix-applydelta-empty → upstream main
```

Then rebase `add-ci` on top of that merged PR (or on upstream/main
after it merges).

## PR #1 should include

1. The `applyDelta` fix (2 lines changed)
2. Removal of `trailingNewline` (7 lines deleted)
3. A focused test — e.g., apply a delta that produces zero output lines
4. Brief commit message explaining the bug