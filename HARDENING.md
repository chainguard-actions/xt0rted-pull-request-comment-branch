<!-- markdownlint-disable -->

# Hardening Report: xt0rted--pull-request-comment-branch/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--pull-request-comment-branch/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions and reusable workflows using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved or compromised.

- ci.yml: `actions/checkout@v3.5.0`, `actions/setup-node@v3.6.0`
- codeql-analysis.yml: `actions/checkout@v3.5.0`, `github/codeql-action/init@v2`, `github/codeql-action/autobuild@v2`, `github/codeql-action/analyze@v2`
- dependabot-auto-merge.yml: `xt0rted/.github/.github/workflows/dependabot-auto-merge.yml@main`
- fixup-commits.yml: `xt0rted/.github/.github/workflows/fixup-commits.yml@main`

Locations:

- `.github/workflows/ci.yml:26`
- `.github/workflows/ci.yml:29`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/codeql-analysis.yml:34`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/dependabot-auto-merge.yml:9`
- `.github/workflows/fixup-commits.yml:7`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` key and its only job (`build`) also has no job-level `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files by resolving each tag/branch to its full 40-character commit SHA (preserving the original tag as a comment). Specifically: actions/checkout@v3.5.0 → @8f4b7f84864484a7bf31766abe9204da3cbe65b3, actions/setup-node@v3.6.0 → @64ed1c7eab4cce3362f8c340dee64e5eaeef8f7c, github/codeql-action/{init,autobuild,analyze}@v2 → @b8d3b6e8af63cde30bdc382c0bc28114f4346c88, and xt0rted/.github reusable workflows@main → @cb2d8c639d7292c04de6e740bf557e7b8f751f7f. Also added a top-level `permissions: contents: read` block to ci.yml to satisfy the least-privilege requirement.

