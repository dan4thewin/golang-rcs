# Split co.go bugfix into separate PR

**Severity:** High (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

The `applyDelta` empty-output fix and `trailingNewline` removal in `co.go` are logically independent of the `ci` feature. Bundling them makes review harder.

**Fix:** Submit as a small preliminary PR:
1. `applyDelta` empty output fix
2. Dead `trailingNewline` removal
3. Focused regression test

Then rebase `ci` feature on top.