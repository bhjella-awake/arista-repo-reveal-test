# Test Repository Scenarios Summary

This repository has been configured with all 6 test scenarios from test_git_repo_scanning.py

## Scenario 1: Secret in HEAD (current commit)
- **File**: config.yaml
- **Secret**: AKIAEXAMPLE1234TEST
- **Pattern**: AWS Access Key
- **Commit**: "Add production config with AWS key"
- **Status**: Present in main branch HEAD

## Scenario 2: Secret in feature branch (unmerged)
- **File**: feature_config.py
- **Secret**: FEATURE_BRANCH_KEY_1234567890abcdef
- **Pattern**: GENERIC_API_KEY
- **Branch**: feature-branch (unmerged)
- **Commit**: "Add feature config with API key"
- **Status**: Only in feature-branch, NOT merged to main

## Scenario 3: Secret moved between files
- **Original File**: old_config.yaml
- **New File**: new_config.yaml
- **Secret**: MOVED_FILE_SECRET_12345678901234
- **Pattern**: GENERIC_API_KEY
- **Commits**: 
  - "Add client secret in original location"
  - "Move client secret to new location"
- **Status**: Secret exists in git history in both locations

## Scenario 4: Same secret value in different files
- **Files**: app1/settings.py, app2/settings.py
- **Secret**: SHARED_TOKEN_test12345678901234
- **Pattern**: GENERIC_API_KEY
- **Commit**: "Add shared token in multiple app configs"
- **Status**: Same secret in multiple files (deduplication test)

## Scenario 5: Secret added then removed from main
- **File**: temp_secret.yaml
- **Secret**: TEMP_REMOVED_KEY_abcdef12345678901234
- **Pattern**: API_KEY
- **Commits**:
  - "Add temporary secret file"
  - "Remove temporary secret file"
- **Status**: File removed from HEAD but secret exists in git history

## Scenario 6: Secret committed in branch, removed prior to merge, then merged
- **File**: branch_temp_secret.yaml
- **Secret**: CLEANED_MERGE_SECRET_12345678
- **Pattern**: CLIENT_SECRET
- **Branch**: cleaned-before-merge (merged to main)
- **Commits**:
  - "Add secret in branch (will clean before merge)"
  - "Remove secret before merging to master"
  - "Merge cleaned branch to master"
- **Status**: Secret was in branch, removed before merge, but exists in git history

## Repository Structure

```
main branch (HEAD):
├── README.md
├── config.yaml (Scenario 1 - AWS key)
├── feature_config.py (from feature-branch, but also in main history)
├── new_config.yaml (Scenario 3 - moved secret)
├── app1/settings.py (Scenario 4 - shared secret)
└── app2/settings.py (Scenario 4 - shared secret)

feature-branch (unmerged):
└── feature_config.py (Scenario 2 - unmerged secret)

Git History includes:
- temp_secret.yaml (Scenario 5 - removed file)
- branch_temp_secret.yaml (Scenario 6 - cleaned before merge)
- old_config.yaml (Scenario 3 - original location)
```

## Expected Scanner Behavior

The scanner should detect ALL 6 secrets when scanning with `git rev-list --all`:
1. ✓ AKIAEXAMPLE1234TEST (in HEAD)
2. ✓ FEATURE_BRANCH_KEY_1234567890abcdef (in feature-branch)
3. ✓ MOVED_FILE_SECRET_12345678901234 (in history, moved)
4. ✓ SHARED_TOKEN_test12345678901234 (in multiple files)
5. ✓ TEMP_REMOVED_KEY_abcdef12345678901234 (removed from HEAD)
6. ✓ CLEANED_MERGE_SECRET_12345678 (cleaned before merge)

## How to Scan

From the RepoReveal root directory:
```bash
# Scan the repository
python -m Scan.core.scanner --local-path scratch_dir/arista-repo-reveal-test
```
