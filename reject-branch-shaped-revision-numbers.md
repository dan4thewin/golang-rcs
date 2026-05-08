# Reject branch-shaped revision numbers

**Severity:** Blocker (GPT-5.5)
**Models:** GPT-5.5

Explicit `WithRevision("1.1.1.1")` would corrupt the revision graph. The implementation only prepends a new head without updating `RevisionHead.Branches`.

**Fix:** Either:
1. Reject non-trunk revisions with a clear error (recommended for now)
2. Implement proper branch checkin semantics

Add tests for branch revision rejection.