# Run gofmt on all changed files

**Severity:** Blocker (GPT-5.5)
**Models:** GPT-5.5

`gofmt -l` reports `cmd/gorcs/ci.go` is not formatted. The diff is small (`lockRevision` field alignment) but it's a basic Go hygiene failure.

**Fix:** Run `gofmt -w` on all changed Go files.