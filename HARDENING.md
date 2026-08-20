<!-- markdownlint-disable -->

# Hardening Report: andstor--file-existence-action/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **andstor--file-existence-action/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference actions by mutable tag rather than a full 40-character commit SHA. If the tag is moved (intentionally or by a supply-chain attacker), the workflow will silently execute different code. Affected references: `actions/checkout@v4` (build.yml) and `Actions-R-Us/actions-tagger@v2.0.3` (versioning.yml). Pin each to its full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build.yml:12`
- `.github/workflows/versioning.yml:9`

### missing-permissions (severity: medium)

Neither `build.yml` nor `versioning.yml` declares a `permissions:` block at the top level or on any job. Without explicit permissions, workflows inherit the repository default (often `write-all`), granting unnecessary access to the GITHUB_TOKEN. Add a minimal `permissions:` block (e.g. `contents: read`) to each workflow or each job.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/versioning.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v4 to full SHA @11d5960a326750d5838078e36cf38b85af677262 in build.yml; (2) Pinned Actions-R-Us/actions-tagger@v2.0.3 to full SHA @330ddfac760021349fef7ff62b372f2f691c20fb in versioning.yml; (3) Added `permissions: contents: read` to build.yml; (4) Added `permissions: contents: write` to versioning.yml (required for the tagger action to create/update git tags). Original tag names preserved as inline comments.

