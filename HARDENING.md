<!-- markdownlint-disable -->

# Hardening Report: xt0rted--pull-request-comment-branch/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--pull-request-comment-branch/v1.3.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in ci.yml are pinned to version tags instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved: `actions/checkout@v2.3.4` (line 19) and `actions/setup-node@v2.1.5` (line 22).

Locations:

- `.github/workflows/ci.yml:19`
- `.github/workflows/ci.yml:22`

### unpinned-uses (severity: high)

The `uses:` reference in pull_request.yml is pinned to a version tag instead of a full 40-character commit SHA: `xt0rted/block-autosquash-commits-action@v2.1.0` (line 13). A mutable tag can be silently redirected to malicious code.

Locations:

- `.github/workflows/pull_request.yml:13`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and the `build` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository default (often `write-all` for private repos), granting broader access than necessary.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

pull_request.yml has no top-level `permissions:` key and the `message-check` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository default, granting broader access than necessary.

Locations:

- `.github/workflows/pull_request.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**ci.yml:**
- Pinned `actions/checkout@v2.3.4` → `@5a4ac9002d0be2fb38bd78e4b4dbde5606d7042f # v2.3.4`
- Pinned `actions/setup-node@v2.1.5` → `@46071b5c7a2e0c34e49c3cb8a0e792e86e18d5ea # v2.1.5`
- Added top-level `permissions: contents: read` (minimum needed for checkout)

**pull_request.yml:**
- Pinned `xt0rted/block-autosquash-commits-action@v2.1.0` → `@cd70ede293e6a8a8c1ddf0dcd3f5858b184cc56a # v2.1.0`
- Added top-level `permissions: contents: read, pull-requests: read` (minimum needed to read PR commit messages)

All SHAs were resolved via `lookup_action_sha` — no values were guessed.

