<!-- markdownlint-disable -->

# Hardening Report: andstor--file-existence-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **andstor--file-existence-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference actions using mutable tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved or hijacked.
- `.github/workflows/build.yml` line 13: `uses: actions/checkout@v1` (tag `v1`)
- `.github/workflows/versioning.yml` line 11: `uses: Actions-R-Us/actions-tagger@latest` (tag `latest`)

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/versioning.yml:11`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job within either file declares job-level permissions. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Minimal required permissions should be declared explicitly.
- `.github/workflows/build.yml`: no `permissions:` key at top level or job level
- `.github/workflows/versioning.yml`: no `permissions:` key at top level or job level

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and Actions-R-Us/actions-tagger@latest to SHA 330ddfac760021349fef7ff62b372f2f691c20fb, preserving original tags as comments. (2) Added top-level `permissions: {}` to both workflow files to deny all permissions by default. For versioning.yml, added job-level `permissions: contents: write` since the actions-tagger action requires it to create/update tags on releases.

