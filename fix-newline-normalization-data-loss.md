# Fix newline normalization data loss

**Severity:** Blocker (GPT-5.5)
**Models:** GPT-5.5

Checking in a file without a final newline silently appends one, changing file content. Real RCS can store text without trailing newlines.

**Evidence:** `ci.go` lines 98-100 and 146-148:
```go
    text += "\\n"
}
```

**Fix:** Preserve exact input text. Fix delta/apply model to handle no-trailing-newline rather than normalizing user content.