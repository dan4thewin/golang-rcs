# Add end-to-end CLI tests

**Severity:** Medium (2/3)
**Models:** GPT-5.5, Sonnet

## Problem

CLI tests inject `CommandAction` rather than testing real file effects
through the command execution path. The code's own AGENTS.md says:
"prefer integration tests that verify behavior through the command
execution path."

Current `cmd/gorcs/ci_test.go` verifies flag parsing by replacing the
action callback. It never touches the filesystem.

## Implementation

Add a test file (e.g., `cmd/gorcs/ci_integration_test.go`) that:

1. Creates a temp dir with a working file
2. Runs `root.Execute([]string{"ci", "-l", "-m", "initial", "file.txt"})`
3. Verifies `file.txt,v` was created with correct content
4. Runs a second checkin and verifies revision increment
5. Tests file disposition:
   - Default (no -l/-u): working file removed
   - `-l`: working file kept, writable
   - `-u`: working file kept, read-only

## Test cases needed

```go
func TestCiIntegration_InitialCheckin(t *testing.T)     // creates archive
func TestCiIntegration_SecondRevision(t *testing.T)     // increments, delta stored
func TestCiIntegration_LockDisposition(t *testing.T)    // -l keeps writable
func TestCiIntegration_UnlockDisposition(t *testing.T)  // -u keeps read-only
func TestCiIntegration_RemoveDisposition(t *testing.T)  // default removes file
func TestCiIntegration_ForceUnchanged(t *testing.T)     // -f with same content
func TestCiIntegration_ConflictingFlags(t *testing.T)   // -l -u → error
```

## Pattern to follow

Look at how upstream tests `cmd/gorcs/co.go` integration — likely
uses `t.TempDir()`, writes a test RCS file, runs the command, and
checks filesystem state after.