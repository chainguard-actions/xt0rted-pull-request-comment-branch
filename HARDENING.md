<!-- markdownlint-disable -->

# Hardening Report: xt0rted--pull-request-comment-branch/v1.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--pull-request-comment-branch/v1.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of full 40-character commit SHA hashes, making them vulnerable to supply-chain attacks if the referenced tag or branch is moved.

Failing references:
- ci.yml: `actions/checkout@v3.1.0`, `actions/setup-node@v3.5.1`
- codeql-analysis.yml: `actions/checkout@v3.1.0`, `github/codeql-action/init@v2`, `github/codeql-action/autobuild@v2`, `github/codeql-action/analyze@v2`
- fixup-commits.yml: `xt0rted/.github/.github/workflows/fixup-commits.yml@main`
- dependabot-auto-merge.yml: `xt0rted/.github/.github/workflows/dependabot-auto-merge.yml@main`

All `uses:` references should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3.1.0`.

Locations:

- `.github/workflows/ci.yml:24`
- `.github/workflows/ci.yml:27`
- `.github/workflows/codeql-analysis.yml:28`
- `.github/workflows/codeql-analysis.yml:31`
- `.github/workflows/codeql-analysis.yml:35`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/ fixup-commits.yml:8`
- `.github/workflows/dependabot-auto-merge.yml:9`

### missing-permissions (severity: medium)

The workflow file `ci.yml` has no top-level `permissions:` key and none of its jobs define a `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access to contents and other scopes). A minimal `permissions:` block should be added at the top level or on each job.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full 40-character commit SHAs in all four workflow files: ci.yml, codeql-analysis.yml, fixup-commits.yml, and dependabot-auto-merge.yml. Added a top-level `permissions: contents: read` block to ci.yml to satisfy the missing-permissions finding. All original tags are preserved as inline comments for readability.

