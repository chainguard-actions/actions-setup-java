<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v5.7.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Every `uses:` reference across all workflow files is pinned to a mutable tag or branch rather than a full 40-character commit SHA. This exposes the workflows to supply-chain attacks where a tag can be silently moved to point to malicious code. Affected references include: `actions/reusable-workflows/...@main`, `actions/checkout@v7`, `actions/setup-python@v6`, `github/codeql-action/upload-sarif@v4`, `actions/publish-immutable-action@v0.0.4`, `actions/publish-action@v0.4.0`, and `actions/reusable-workflows/.github/workflows/update-config-files.yml@main`. All should be replaced with pinned SHA digests.

Locations:

- `.github/workflows/basic-validation.yml:17`
- `.github/workflows/check-dist.yml:16`
- `.github/workflows/codeql-analysis.yml:17`
- `.github/workflows/e2e-cache-dependency-path.yml:27`
- `.github/workflows/e2e-cache.yml:26`
- `.github/workflows/e2e-local-file.yml:27`
- `.github/workflows/e2e-publishing.yml:28`
- `.github/workflows/e2e-versions.yml:84`
- `.github/workflows/licensed.yml:14`
- `.github/workflows/publish-immutable-actions.yml:17`
- `.github/workflows/release-new-action-version.yml:22`
- `.github/workflows/update-config-files.yml:17`
- `.github/workflows/zizmor.yml:24`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned `uses:` references across 13 workflow files to full 40-character commit SHAs:

- `actions/reusable-workflows@main` → `@9e7901ee9359b9b3e7e990fcc499e43aa005eec3 # main` (in basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml)
- `actions/checkout@v7` → `@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7` (in e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, publish-immutable-actions.yml, zizmor.yml)
- `actions/checkout@v6` → `@d23441a48e516b6c34aea4fa41551a30e30af803 # v6` (in e2e-versions.yml setup-java-set-default job)
- `actions/setup-python@v6` → `@ece7cb06caefa5fff74198d8649806c4678c61a1 # v6` (in zizmor.yml)
- `github/codeql-action/upload-sarif@v4` → `@f205ea1c3313d32999d8d6a48b4f6530d4437b38 # v4` (in zizmor.yml)
- `actions/publish-immutable-action@v0.0.4` → `@4bc8754ffc40f27910afb20287dbbbb675a4e978 # v0.0.4` (in publish-immutable-actions.yml)
- `actions/publish-action@v0.4.0` → `@23f4c6f12633a2da8f44938b71fde9afec138fb4 # v0.4.0` (in release-new-action-version.yml)

All SHAs were resolved using lookup_action_sha. Original tags preserved as inline comments.

