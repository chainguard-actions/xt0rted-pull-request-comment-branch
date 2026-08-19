<!-- markdownlint-disable -->

# Hardening Report: xt0rted--pull-request-comment-branch/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xt0rted--pull-request-comment-branch/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved or the referenced repository is compromised. Failing references: ci.yml uses `actions/checkout@v2.3.2` and `actions/setup-node@v1.4.3`; pull_request.yml uses `xt0rted/block-autosquash-commits-action@v2.0.0`. All should be pinned to their full commit SHA (e.g. `actions/checkout@<40-char-sha> # v2.3.2`).

Locations:

- `.github/workflows/ci.yml:18`
- `.github/workflows/ci.yml:21`
- `.github/workflows/pull_request.yml:11`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job in either file has a job-level `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, which violates the principle of least privilege. A `permissions: {}` or minimal scoped block should be added to each workflow.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/pull_request.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all three action references to full 40-char commit SHAs with original tags preserved as comments — actions/checkout@v2.3.2→2036a08e, actions/setup-node@v1.4.3→4bb8c450, xt0rted/block-autosquash-commits-action@v2.0.0→44e09ef2. (2) Added `permissions: {}` top-level block to both ci.yml and pull_request.yml to enforce least-privilege for the GITHUB_TOKEN.

