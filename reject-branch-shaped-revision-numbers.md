# Reject branch-shaped revision numbers

**Severity:** Blocker (GPT-5.5)
**Models:** GPT-5.5

## Problem

Explicit `WithRevision("1.1.1.1")` would corrupt the revision graph.
The implementation only prepends a new head and sets `NextRevision` to
point at the previous head. For branch revisions, it should instead:
- Find the parent revision (e.g., `1.1` for branch `1.1.1.x`)
- Add to `RevisionHead.Branches` on the parent
- NOT update `file.Head`

## Where

`ci.go` → `checkinNormal()`, after `incrementNum()` / targetRev resolution.

## Implementation (recommended: reject for now)

Add validation after determining `newRev`:

```go
parts := strings.Split(newRev, ".")
if len(parts) > 2 {
    return nil, fmt.Errorf("branch revision %q not supported; only trunk checkins (N.M) are implemented", newRev)
}
```

Also validate in `incrementNum` — if input is a branch revision like
`1.1.1.3`, the incremented `1.1.1.4` would be a branch revision that
we can't safely store. But `incrementNum` is only called when no
explicit revision is given and we're incrementing head, which is always
trunk if head is trunk. So the guard in `checkinNormal` for explicit
revisions is the main concern.

## Tests

- `WithRevision("1.1.1.1")` → clear error message
- `WithRevision("1.1.1.1.1.1")` → rejected
- `WithRevision("1.5")` → accepted (trunk)
- Auto-increment from trunk head → stays on trunk