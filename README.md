# arista-repo-reveal-test

This is a test repository for RepoReveal secret scanning.

## ⚠️ IMPORTANT: Test Secrets Only

**All "secrets" in this repository are fake test values created specifically for testing purposes.**

- These are NOT real credentials, API keys, or sensitive data
- All secret values are clearly marked as test/example values
- This repository is intentionally designed to trigger secret detection patterns
- No actual security risk exists from these test values

## Purpose

This repository contains 6 test scenarios to validate RepoReveal's ability to detect secrets across various git history situations:

1. Secret in HEAD (current commit)
2. Secret in unmerged feature branch
3. Secret moved between files
4. Same secret in multiple files
5. Secret added then removed from main
6. Secret committed in branch, removed before merge

See `SCENARIO_SUMMARY.md` for detailed information about each test scenario.
