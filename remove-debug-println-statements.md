# Remove debug Println statements

**Source:** ProjectsToDo (Apr 2026)
**Status:** Still present

Debug `fmt.Println("Parsing: ", fn)` in:
- `internal/cli/list_heads.go:36`
- `internal/cli/normalize_revisions.go:161`

Also verbose date-update prints in `normalize_revisions.go:69,72,78`.

These are upstream code, not fork additions. Consider whether to include in a cleanup PR or leave for upstream.