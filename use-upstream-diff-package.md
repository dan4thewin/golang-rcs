# Use upstream diff package

**Severity:** Blocker/High (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

`ci.go` has a bespoke `generateDelta()` using prefix/suffix matching. Upstream just added `diff.Generate()` with znkr as default algorithm. The custom implementation ignores upstream direction and can produce larger deltas for interleaved changes.

**Fix:** Replace `generateDelta(newLines, oldLines)` with the shared `diff` package. Keep existing prefix/suffix tests as correctness regression tests.