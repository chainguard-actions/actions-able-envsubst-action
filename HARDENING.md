<!-- markdownlint-disable -->

# Hardening Report: actions-able--envsubst-action/v1.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions-able--envsubst-action/v1.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag/version refs instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved.

`.github/workflows/release.yaml`:
- `uses: actions/checkout@v3` (line ~14)
- `uses: rlespinasse/github-slug-action@v4` (line ~17)
- `uses: cycjimmy/semantic-release-action@v3` (line ~20)

`.github/workflows/test.yml`:
- `uses: actions/checkout@v3` (appears in multiple jobs)

Locations:

- `.github/workflows/release.yaml:14`
- `.github/workflows/release.yaml:17`
- `.github/workflows/release.yaml:20`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

`.github/workflows/test.yml` has no top-level `permissions:` key, and the `docker-build` job (the first job in the file) has no job-level `permissions:` key either. This means the job runs with the default, overly-broad repository token permissions. The other three jobs do have job-level `permissions: contents: write`, but the `docker-build` job is unprotected.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned `uses:` references by resolving them to full commit SHAs:
- `actions/checkout@v3` → `@a37ce9120846195fa4ece8f58b268e6043cb2f26` (applied in both release.yaml and all 4 occurrences in test.yml)
- `rlespinasse/github-slug-action@v4` → `@797d68864753cbceedc271349d402da4590e6302` (release.yaml)
- `cycjimmy/semantic-release-action@v3` → `@8e58d20d0f6c8773181f43eb74d6a05e3099571d` (release.yaml)

Added `permissions: {}` to the `docker-build` job in test.yml, which previously had no permissions block and ran with overly-broad default token permissions. The original tag names are preserved as inline comments for readability.

