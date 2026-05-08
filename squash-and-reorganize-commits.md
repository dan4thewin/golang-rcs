# Squash and reorganize commits

**Severity:** Medium (unanimous)
**Models:** Opus, GPT-5.5, Sonnet

The 8 commits include audit-fix/refactor/testing progression that reads as development history, not PR-ready.

**Fix:** Reorganize into:
1. `fix checkout delta application for empty output` (if not split to separate PR)
2. `add File.Checkin API with delta generation and tests`
3. `add gorcs ci command and CLI tests`
4. `document checkin support in README`