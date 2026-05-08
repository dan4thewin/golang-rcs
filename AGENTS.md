# Agent Instructions

## Workspace Layout

This is the `notes` worktree of `dan4thewin/golang-rcs` — a fork of `arran4/golang-rcs`.

```
~/src/golang-rcs-project/
├── code/   ← branch: add-ci   (feature work, Go source)
└── notes/  ← branch: notes    (orphan, IWE task graph, this dir)
```

Both worktrees share one `.git` database. The `notes` branch is an
orphan (no common ancestor with `main` or `add-ci`).

## Repository Topology

- **Upstream:** `arran4/golang-rcs` (remote: `upstream`, branch: `main`)
- **Fork:** `dan4thewin/golang-rcs` (remote: `origin`)
- **Feature branch:** `add-ci` (8 commits, trunk checkin feature)
- **Fork point:** commit `afc454e` (2026-03-28, upstream PR #203 merge)
- **Upstream activity:** Very active (~621 commits since Jan 2025)

## Key Upstream Architecture

### diff package (`diff/`)

Upstream recently added a pluggable diff engine (merged in PR #203):

```
diff/
├── generate.go      — diff.Generate(from, to []string) (EdDiff, error)
├── register.go      — Registry: Register(), GetAlgorithm(), SetDefaultAlgorithm()
├── types.go         — EdDiff, EdDiffCommand, Delete, Add, LineReader, LineWriter
├── lcs.go           — LCS algorithm (registered as "lcs")
└── znkr_diff/znkr.go — znkr algorithm (set as default)
```

Key API:
```go
import "github.com/arran4/golang-rcs/diff"
_ "github.com/arran4/golang-rcs/diff/znkr_diff"  // registers "znkr"

edDiff, err := diff.Generate(fromLines, toLines)
deltaStr := edDiff.String()  // produces "d3 2\na5 1\nfoo\n" format
```

`EdDiff.String()` outputs the same `dN M` / `aN M` RCS edit-script
format that `co.go` already parses with `applyDelta`.

### Checkout API (`co.go`)

```go
func (file *File) Checkout(user string, ops ...any) (*COVerdict, error)
```

Options: `WithRevision`, `WithDate`, `WithLock` (WithSetLock/WithClearLock/WithNoLockChange).

### Model (`model.go`)

- `NewFile()` sets `Strict: true` by default
- `File.Head` is the head revision string
- `RevisionHead` has `Revision`, `Date`, `Author`, `State`, `NextRevision`, `Branches`
- Locks stored in `File.Locks` (map of user→revision)

## Task Graph

Use IWE to navigate:
```bash
iwe tree -k pr-readiness-audit -d 2     # overview
iwe retrieve -k <task-key> -d 0         # read a specific task
```

## Working in the Code Worktree

```bash
cd ../code                    # switch to code
git --no-pager branch         # confirm on add-ci
go test ./...                 # run tests
```

## PR Submission Plan

1. **PR #1** (bugfix): `co.go` applyDelta empty-output fix + dead `trailingNewline` removal
2. **PR #2** (feature): `ci` checkin with all blocker fixes applied

Before either PR: rebase onto `upstream/main`, resolve conflicts.
