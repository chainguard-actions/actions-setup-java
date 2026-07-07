<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--setup-java/v5.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags/branches instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the branch is updated. Failing references include: actions/checkout@v6 (all e2e workflows), actions/reusable-workflows/...@main (basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml), actions/publish-immutable-action@v0.0.4 (publish-immutable-actions.yml), actions/publish-action@v0.4.0 (release-new-action-version.yml).

Locations:

- `.github/workflows/basic-validation.yml:16`
- `.github/workflows/check-dist.yml:15`
- `.github/workflows/codeql-analysis.yml:14`
- `.github/workflows/e2e-cache-dependency-path.yml:22`
- `.github/workflows/e2e-cache.yml:22`
- `.github/workflows/e2e-local-file.yml:23`
- `.github/workflows/e2e-publishing.yml:24`
- `.github/workflows/e2e-versions.yml:68`
- `.github/workflows/licensed.yml:13`
- `.github/workflows/publish-immutable-actions.yml:17`
- `.github/workflows/release-new-action-version.yml:28`
- `.github/workflows/update-config-files.yml:13`

### script-injection (severity: high)

Multiple run: blocks directly interpolate GitHub Actions expressions (${{ ... }}) into shell command strings, violating rule (a). This allows expression values to be interpreted as shell code before the shell ever sees them. Offending lines include: `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"` in e2e-versions.yml (multiple jobs), and `run: bash __tests__/verify-java.sh "11.0.10" "${{ steps.setup-java.outputs.path }}"` in e2e-local-file.yml. The `steps.setup-java.outputs.path` and `matrix.version` values are workflow-controlled and should be passed via env vars with double-quoted shell expansions instead.

Locations:

- `.github/workflows/e2e-versions.yml:76`
- `.github/workflows/e2e-versions.yml:100`
- `.github/workflows/e2e-versions.yml:130`
- `.github/workflows/e2e-local-file.yml:43`
- `.github/workflows/e2e-local-file.yml:72`
- `.github/workflows/e2e-local-file.yml:101`

### missing-permissions (severity: medium)

Multiple workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows inherit the default repository permissions (which may be read/write for contents), violating the principle of least privilege.

Locations:

- `.github/workflows/basic-validation.yml:1`
- `.github/workflows/check-dist.yml:1`
- `.github/workflows/codeql-analysis.yml:1`
- `.github/workflows/e2e-cache-dependency-path.yml:1`
- `.github/workflows/e2e-cache.yml:1`
- `.github/workflows/e2e-local-file.yml:1`
- `.github/workflows/e2e-publishing.yml:1`
- `.github/workflows/e2e-versions.yml:1`
- `.github/workflows/licensed.yml:1`
- `.github/workflows/update-config-files.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings across 12 workflow files:

1. unpinned-uses: Pinned all action references to full SHA hashes:
   - actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10 (all e2e workflows)
   - actions/reusable-workflows@main → @09976383aa8780d306ee271bd21bb77a54fad474 (basic-validation, check-dist, codeql-analysis, licensed, update-config-files)
   - actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978
   - actions/publish-action@v0.4.0 → @23f4c6f12633a2da8f44938b71fde9afec138fb4

2. script-injection: Moved all ${{ steps.setup-java.outputs.path }} and ${{ matrix.version }} expressions out of run: shell strings into env: blocks, referencing them as $SETUP_JAVA_PATH and $MATRIX_VERSION respectively. Fixed in e2e-versions.yml (multiple jobs) and e2e-local-file.yml (3 jobs).

3. missing-permissions: Added top-level 'permissions: {}' to all 10 workflow files that lacked permissions blocks (basic-validation, check-dist, codeql-analysis, e2e-cache-dependency-path, e2e-cache, e2e-local-file, e2e-publishing, e2e-versions, licensed, update-config-files). The publish-immutable-actions.yml already had job-level permissions and release-new-action-version.yml already had top-level permissions: contents: write — both were preserved.

