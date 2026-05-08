# Fix file permissions in CLI checkin

**Severity:** Low (GPT-5.5)
**Models:** GPT-5.5

`ciFile` always writes archives as `0644` then chmods to `0444`. This strips group bits, execute bits, or custom modes. Existing checkout code preserves permissions more carefully.

**Fix:** Stat existing files and preserve base permissions, toggling only write bits according to lock/unlock disposition.