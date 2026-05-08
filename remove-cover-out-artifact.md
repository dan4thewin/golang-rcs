# Remove cover.out artifact

**Severity:** Blocker (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

`cover.out` (3555 lines of Go coverage output) is committed. Upstream will reject this as accidental generated output.

**Fix:** Remove from tree, add `cover.out` to `.gitignore`.