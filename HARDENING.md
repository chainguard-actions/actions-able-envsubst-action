<!-- markdownlint-disable -->

# Hardening Report: actions-able--envsubst-action/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions-able--envsubst-action/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable version tags rather than immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised.

In .github/workflows/release.yaml:
- `uses: actions/checkout@v3` (tag, not SHA)
- `uses: rlespinasse/github-slug-action@v4` (tag, not SHA)
- `uses: cycjimmy/semantic-release-action@v3` (tag, not SHA)

In .github/workflows/test.yml:
- `uses: actions/checkout@v3` (tag, not SHA — appears in multiple jobs)

Locations:

- `.github/workflows/release.yaml:16`
- `.github/workflows/release.yaml:20`
- `.github/workflows/release.yaml:23`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:43`
- `.github/workflows/test.yml:62`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yml has no top-level `permissions:` block, and the `docker-build` job does not define its own `permissions:` key. This means the docker-build job runs with the default (broad) GitHub token permissions. The other three jobs (test-action-with-input-file, test-action-with-input-pattern, test-entrypoint-script) each have job-level `permissions:` blocks, but docker-build does not, leaving it without explicit least-privilege permissions.

Locations:

- `.github/workflows/test.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving them to full commit SHAs: actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, rlespinasse/github-slug-action@v4 → 797d68864753cbceedc271349d402da4590e6302, cycjimmy/semantic-release-action@v3 → 8e58d20d0f6c8773181f43eb74d6a05e3099571d. Original tags preserved as inline comments. Added `permissions: {}` to the docker-build job in test.yml, which only runs `make docker-build` and requires no GitHub token permissions.

