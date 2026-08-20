<!-- markdownlint-disable -->

# Hardening Report: andstor--file-existence-action/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **andstor--file-existence-action/v3.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference external actions using mutable tags instead of pinned full-length commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved or the upstream repository is compromised. Affected references: `actions/checkout@v4` (build.yml line 13) and `Actions-R-Us/actions-tagger@v2.0.3` (versioning.yml line 11). Each should be replaced with the corresponding 40-character hex commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/versioning.yml:11`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job within either file declares job-level permissions. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions, violating the principle of least privilege. A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be added at the top level or per job in both build.yml and versioning.yml.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v4 to SHA 11d5960a326750d5838078e36cf38b85af677262 in build.yml, and Actions-R-Us/actions-tagger@v2.0.3 to SHA 330ddfac760021349fef7ff62b372f2f691c20fb in versioning.yml, preserving original tags as comments. (2) Added `permissions: {}` to build.yml (no token permissions needed) and `permissions: contents: write` to versioning.yml (required for the tagger action to push/update git tags).

