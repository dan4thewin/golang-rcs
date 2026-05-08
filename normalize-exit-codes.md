# Normalize exit codes

**Source:** ProjectsToDo (Apr 2026)
**Status:** Still relevant

`cmd/gorcs/main.go` uses `ErrExitCode` pattern which is fine, but individual subcommands (ci, co) return errors without distinguishing user-error (exit 1) from internal failure (exit 2). Standard RCS uses:
- 0: success
- 1: user error / no change needed
- 2: internal error

Check whether upstream cares about this convention.