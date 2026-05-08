# Add end-to-end CLI tests

**Severity:** Medium (2/3)
**Models:** GPT-5.5, Sonnet

CLI tests inject `CommandAction` rather than testing real file effects through the command execution path. AGENTS.md says: "prefer integration tests that verify behavior through the command execution path."

**Fix:** Add end-to-end tests:
- initial `gorcs ci -u`
- second revision `gorcs ci -l`
- `-fREV` force checkin
- conflicting flags error
- file disposition/remove behavior