# Fix newline normalization data loss

**Severity:** Blocker (GPT-5.5)
**Models:** GPT-5.5

## Problem

Checking in a file without a final newline silently appends one,
changing file content. Real RCS can store text without trailing newlines.

In `ci.go` (both `checkinInitial` ~line 98 and `checkinNormal` ~line 146):
```go
if text != "" && !strings.HasSuffix(text, "\n") {
    text += "\n"
}
```

## Why it exists

The delta/apply model in `co.go` uses `splitLines()` which splits on `\n`
and drops empty trailing elements. Without normalization, a file ending
without `\n` would lose its last line boundary distinction after a
round-trip through delta generation and application.

## Fix approach

Two options:

**Option A (minimal):** Keep normalization but document it clearly as
an intentional RCS convention. Traditional RCS also normalizes to
trailing newline. This is arguably correct behavior. Add a godoc note
and test that demonstrates the normalization is intentional.

**Option B (full fidelity):** Track whether text has a trailing newline
via an `@` marker or similar convention in the stored delta, and
reconstruct exactly on checkout. This is complex and non-standard.

**Recommendation:** Option A is likely acceptable for upstream since
traditional `ci(1)` also normalizes. But document it and don't claim
no-trailing-newline support in README. The key is the README shouldn't
promise exact byte preservation if we normalize.

## Interaction with other tasks

- Ties to [Narrow README claims to current scope](narrow-readme-claims-to-current-scope)
- Existing `TestNewlineNormalizationRoundTrip` tests already validate the current behavior