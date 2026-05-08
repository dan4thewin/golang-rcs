# Enforce strict-lock semantics

**Severity:** Blocker (2/3)
**Models:** GPT-5.5, Sonnet

## Problem

`Checkin`/`checkinNormal` never validates lock ownership. With strict
locking (`file.Strict == true`, which is the default from `NewFile()`),
any user can check in without holding the lock. This is the core safety
mechanism of RCS workflows.

## Where

`ci.go` → `checkinNormal()`, around line 138 (before resolving head content).

## Implementation

Add before the `headText` resolution:

```go
if file.Strict {
    // Check caller holds the lock on head
    lockHolder := ""
    for _, lock := range file.Locks {
        if lock.Revision.String() == file.Head {
            lockHolder = string(lock.Author)
            break
        }
    }
    if lockHolder == "" {
        return nil, fmt.Errorf("checkin aborted: %s is not locked (strict locking)", file.Head)
    }
    if lockHolder != user {
        return nil, fmt.Errorf("checkin aborted: %s is locked by %s, not %s", file.Head, lockHolder, user)
    }
}
```

Note: Check how `File.Locks` is structured — it may be a slice of
`Lock{Author, Revision}` or a different shape. Look at `model.go`
and how `co.go` checks locks in `isLockedBy()`.

## Tests needed

- strict archive, no lock on head → error
- strict archive, lock held by same user → success
- strict archive, lock held by different user → error
- non-strict archive (`file.Strict = false`) → checkin without lock succeeds
- `WithForce` behavior — decide if it should bypass lock check (traditional `rcs ci -f` does NOT bypass locks)