# Narrow README claims to current scope

**Severity:** Low (2/3)
**Models:** GPT-5.5, Sonnet

README additions imply broader RCS support than the implementation provides. Branch checkins, strict-lock enforcement, and exact no-trailing-newline preservation are not yet supported.

**Fix:** Add scope note like "basic trunk checkin support" or fix the limitations before advertising full checkin support.