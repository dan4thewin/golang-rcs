# Use upstream diff package

**Severity:** Blocker/High (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

## Problem

`ci.go` has a bespoke `generateDelta()` using prefix/suffix matching.
It finds common prefix + suffix and emits a single delete+add for
the middle. This produces at most one hunk, which means larger deltas
for interleaved changes:

```
Old: a X b Y c    →  Your algo: d2 3 / a4 3 (replace entire middle)
New: a P b Q c    →  LCS algo:  d2 1 / a2 1 + d4 1 / a4 1 (two hunks)
```

Upstream just added `diff.Generate()` with znkr as default in PR #203.
Submitting a PR that ignores this new infrastructure will draw immediate
review pushback.

## Implementation

In `ci.go`, replace:

```go
newLines := splitLines(text)
oldLines := splitLines(headText)
deltaStr := generateDelta(newLines, oldLines)
```

With:

```go
import (
    "github.com/arran4/golang-rcs/diff"
    _ "github.com/arran4/golang-rcs/diff/znkr_diff"
)

newLines := splitLines(text)
oldLines := splitLines(headText)
// Reverse delta: transforms new text → old (head) text
edDiff, err := diff.Generate(newLines, oldLines)
if err != nil {
    return nil, fmt.Errorf("generate delta: %w", err)
}
deltaStr := edDiff.String()
```

Key points:
- `diff.Generate(from, to)` produces a forward diff (from→to)
- For checkin we need a reverse delta (new→old), so call `diff.Generate(newLines, oldLines)`
- `EdDiff.String()` outputs `dN M\naN M\nline\n...` format — same as what `applyDelta` in `co.go` already parses
- The znkr algorithm is registered via blank import of `diff/znkr_diff`

## After

- Remove `generateDelta()` function from `ci.go`
- Keep `TestGenerateDeltaUnit` and `TestGenerateDeltaInterleaved` — convert them to test `diff.Generate` directly, or keep as round-trip regression tests
- Run `go test ./...` — existing round-trip tests validate the integration