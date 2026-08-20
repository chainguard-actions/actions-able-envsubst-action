<!-- markdownlint-disable -->

# Hardening Report: actions-able--envsubst-action/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions-able--envsubst-action/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow file use mutable tag-based refs instead of pinned 40-character SHA commits, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved or compromised. Failing references: `actions/checkout@v4` (lines 14, 24, 43, 62, 70) and `rlespinasse/release-that@v1` (line 79).

Locations:

- `.github/workflows/test-and-release.yml:14`
- `.github/workflows/test-and-release.yml:24`
- `.github/workflows/test-and-release.yml:43`
- `.github/workflows/test-and-release.yml:62`
- `.github/workflows/test-and-release.yml:70`
- `.github/workflows/test-and-release.yml:79`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and the `docker-build` job has no job-level `permissions:` block. This means the docker-build job runs with the default (potentially broad) GITHUB_TOKEN permissions. Every job must either have its own `permissions:` block or the file must declare a restrictive top-level `permissions:` key.

Locations:

- `.github/workflows/test-and-release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 6 `uses:` references to full 40-character SHA commits (actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, rlespinasse/release-that@v1 → @f4912d4053839003bb368e9c7067b071ccb1c146). Added top-level `permissions: {}` to restrict default GITHUB_TOKEN permissions across all jobs, and added an explicit `permissions: {}` block to the `docker-build` job which previously had no permissions block.

