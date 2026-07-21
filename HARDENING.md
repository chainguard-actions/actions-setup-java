<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v5.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of immutable full-length SHA digests, making them vulnerable to supply-chain attacks. Failing references include: basic-validation.yml uses actions/reusable-workflows@main; check-dist.yml uses actions/reusable-workflows@main; codeql-analysis.yml uses actions/reusable-workflows@main; licensed.yml uses actions/reusable-workflows@main; update-config-files.yml uses actions/reusable-workflows@main; publish-immutable-actions.yml uses actions/checkout@v7 and actions/publish-immutable-action@v0.0.4; release-new-action-version.yml uses actions/publish-action@v0.4.0; zizmor.yml uses actions/checkout@v6, actions/setup-python@v5, and github/codeql-action/upload-sarif@v3; e2e-cache-dependency-path.yml uses actions/checkout@v7 (3x); e2e-cache.yml uses actions/checkout@v7 (6x); e2e-local-file.yml uses actions/checkout@v7 (3x); e2e-publishing.yml uses actions/checkout@v7 (4x); e2e-versions.yml uses actions/checkout@v7 (14x). All uses: references must be pinned to a full 40-character commit SHA.

Locations:

- `.github/workflows/basic-validation.yml:19`
- `.github/workflows/check-dist.yml:18`
- `.github/workflows/codeql-analysis.yml:17`
- `.github/workflows/licensed.yml:16`
- `.github/workflows/update-config-files.yml:16`
- `.github/workflows/publish-immutable-actions.yml:18`
- `.github/workflows/release-new-action-version.yml:22`
- `.github/workflows/zizmor.yml:22`
- `.github/workflows/e2e-cache-dependency-path.yml:26`
- `.github/workflows/e2e-cache.yml:24`
- `.github/workflows/e2e-local-file.yml:26`
- `.github/workflows/e2e-publishing.yml:27`
- `.github/workflows/e2e-versions.yml:68`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned action references to full 40-character commit SHAs across 13 workflow files:
- actions/reusable-workflows@main → @4735e71081024a944852f4ab9d1495b6dd2de8f2 (5 files: basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml)
- actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1 (6 files: publish-immutable-actions.yml, e2e-cache-dependency-path.yml x3, e2e-cache.yml x6, e2e-local-file.yml x3, e2e-publishing.yml x4, e2e-versions.yml x14)
- actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 (publish-immutable-actions.yml)
- actions/publish-action@v0.4.0 → @23f4c6f12633a2da8f44938b71fde9afec138fb4 (release-new-action-version.yml)
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 (zizmor.yml)
- actions/setup-python@v5 → @a26af69be951a213d495a4c3e4e4022e16d87065 (zizmor.yml)
- github/codeql-action/upload-sarif@v3 → @08d09a53f0f5d694f253bd25732e4429c9e9337f (zizmor.yml)
All original tag names preserved as inline comments for readability.

