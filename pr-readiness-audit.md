# PR Readiness Audit

Three-model audit of checkin feature PR-readiness (Opus, GPT-5.5, Sonnet). Findings below ordered by severity.

## Blockers

[Remove cover.out artifact](remove-cover-out-artifact)

[Enforce strict-lock semantics](enforce-strict-lock-semantics)

[Use upstream diff package](use-upstream-diff-package)

[Run gofmt on all changed files](run-gofmt-on-all-changed-files)

[Reject branch-shaped revision numbers](reject-branch-shaped-revision-numbers)

[Fix newline normalization data loss](fix-newline-normalization-data-loss)

## High

[Split co.go bugfix into separate PR](split-co-go-bugfix-into-separate-pr)

[Squash and reorganize commits](squash-and-reorganize-commits)

## Medium

[Verify generated banner in cmd gorcs ci](verify-generated-banner-in-cmd-gorcs-ci)

[Add end-to-end CLI tests](add-end-to-end-cli-tests)

## Low

[Fix file permissions in CLI checkin](fix-file-permissions-in-cli-checkin)

[Narrow README claims to current scope](narrow-readme-claims-to-current-scope)