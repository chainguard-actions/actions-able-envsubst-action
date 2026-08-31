<!-- markdownlint-disable -->

# Hardening Report: actions-able--envsubst-action/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions-able--envsubst-action/v1.2.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses mutable tag-based action references instead of pinned 40-character SHA commits. If the referenced tag is moved (e.g. by a supply-chain compromise), the workflow will silently execute different code. Failing references: `actions/checkout@v4` and `github/super-linter@v7`.

Locations:

- `.github/workflows/linter.yml:14`
- `.github/workflows/linter.yml:19`

### unpinned-uses (severity: high)

The workflow uses mutable tag-based action references instead of pinned 40-character SHA commits. If the referenced tag is moved (e.g. by a supply-chain compromise), the workflow will silently execute different code. Failing references: `actions/checkout@v4` (multiple steps) and `rlespinasse/release-that@v1`.

Locations:

- `.github/workflows/test-and-release.yml:16`
- `.github/workflows/test-and-release.yml:26`
- `.github/workflows/test-and-release.yml:46`
- `.github/workflows/test-and-release.yml:63`
- `.github/workflows/test-and-release.yml:88`
- `.github/workflows/test-and-release.yml:96`

### broad-permissions (severity: medium)

The top-level `permissions: read-all` grants read access to all scopes across every job in the workflow. This is overly broad; permissions should be set to the minimal specific scopes actually required (e.g. `contents: read`).

Locations:

- `.github/workflows/linter.yml:5`

### broad-permissions (severity: medium)

The top-level `permissions: read-all` grants read access to all scopes across every job in the workflow. This is overly broad; permissions should be set to the minimal specific scopes actually required by each job.

Locations:

- `.github/workflows/test-and-release.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, broad-permissions

**Notes:**

Fixed all 4 findings across 2 workflow files:

1. linter.yml - unpinned-uses: Pinned `actions/checkout@v4` → SHA `11d5960a326750d5838078e36cf38b85af677262` and `github/super-linter@v7` → SHA `b807e99ddd37e444d189cfd2c2ca1274d8ae8ef1`. Both retain their tag as a comment.

2. linter.yml - broad-permissions: Replaced top-level `permissions: read-all` with specific minimal permissions (`contents: read`, `packages: read`, `statuses: write`) that match what the single job actually requires.

3. test-and-release.yml - unpinned-uses: Pinned all 5 occurrences of `actions/checkout@v4` → SHA `11d5960a326750d5838078e36cf38b85af677262` and `rlespinasse/release-that@v1` → SHA `f4912d4053839003bb368e9c7067b071ccb1c146`.

4. test-and-release.yml - broad-permissions: Replaced top-level `permissions: read-all` with `contents: read` as the minimal safe default; individual jobs already define their own specific permissions blocks where elevated access is needed.

