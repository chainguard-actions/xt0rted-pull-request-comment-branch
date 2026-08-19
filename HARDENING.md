<!-- markdownlint-disable -->

# Hardening Report: xt0rted--pull-request-comment-branch/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--pull-request-comment-branch/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions and reusable workflows using mutable version tags or branch names instead of full 40-character commit SHA pins. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to point at malicious code.

- .github/workflows/ci.yml: `actions/checkout@v4.2.2`, `actions/setup-node@v4.1.0`
- .github/workflows/codeql-analysis.yml: `actions/checkout@v4.2.2`, `github/codeql-action/init@v3`, `github/codeql-action/autobuild@v3`, `github/codeql-action/analyze@v3`
- .github/workflows/dependabot-auto-merge.yml: `xt0rted/.github/.github/workflows/dependabot-auto-merge.yml@main`
- .github/workflows/fixup-commits.yml: `xt0rted/.github/.github/workflows/fixup-commits.yml@main`

Locations:

- `.github/workflows/ci.yml:24`
- `.github/workflows/ci.yml:27`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:30`
- `.github/workflows/codeql-analysis.yml:34`
- `.github/workflows/codeql-analysis.yml:37`
- `.github/workflows/dependabot-auto-merge.yml:9`
- `.github/workflows/fixup-commits.yml:8`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/ci.yml` has no top-level `permissions:` key and its only job (`build`) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the repository's default token permissions, which may be overly broad (e.g., `write` access to contents). A minimal permissions block such as `permissions: contents: read` should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 4 workflow files by replacing mutable tags/branches with full 40-character commit SHAs (preserving original tags as comments): actions/checkout@v4.2.2 → @11bd71901bbe5b1630ceea73d27597364c9af683, actions/setup-node@v4.1.0 → @39370e3970a6d050c480ffad4ff0ed4d3fdee5af, github/codeql-action/{init,autobuild,analyze}@v3 → @b7351df727350dca84cb9d725d57dcf5bc82ba26, xt0rted/.github reusable workflows @main → @cb2d8c639d7292c04de6e740bf557e7b8f751f7f. Also added top-level `permissions: contents: read` to ci.yml to restrict the default token permissions.

