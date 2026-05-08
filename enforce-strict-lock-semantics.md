# Enforce strict-lock semantics

**Severity:** Blocker (2/3)
**Models:** GPT-5.5, Sonnet

`Checkin`/`checkinNormal` never validates lock ownership. With strict locking (`file.Strict == true`, which is the default from `NewFile()`), any user can check in without holding the lock.

**Fix:**
- Before normal checkin, check `file.Strict`
- Require caller to own the head lock
- Block checkin when another user holds the lock
- Add negative tests: no lock, wrong user, other-user lock