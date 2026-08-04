<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v4.9.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v4.9.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Multiple run: blocks directly interpolate ${{ ... }} expressions into shell commands. In e2e-local-file.yml, three 'Verify Java version' steps pass ${{ steps.setup-java.outputs.path }} directly into a bash command string. In e2e-versions.yml, multiple 'Verify Java' steps pass both ${{ matrix.version }} and ${{ steps.setup-java.outputs.path }} directly into bash command strings. These expressions are substituted by the YAML template engine before the shell ever sees them, allowing shell metacharacter injection. Example offending lines: `run: bash __tests__/verify-java.sh "11.0.10" "${{ steps.setup-java.outputs.path }}"` and `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"`.

Locations:

- `.github/workflows/e2e-local-file.yml:44`
- `.github/workflows/e2e-local-file.yml:72`
- `.github/workflows/e2e-local-file.yml:100`
- `.github/workflows/e2e-versions.yml:62`
- `.github/workflows/e2e-versions.yml:100`
- `.github/workflows/e2e-versions.yml:131`
- `.github/workflows/e2e-versions.yml:163`
- `.github/workflows/e2e-versions.yml:196`
- `.github/workflows/e2e-versions.yml:218`
- `.github/workflows/e2e-versions.yml:240`
- `.github/workflows/e2e-versions.yml:320`
- `.github/workflows/e2e-versions.yml:347`
- `.github/workflows/e2e-versions.yml:374`
- `.github/workflows/e2e-versions.yml:401`
- `.github/workflows/e2e-versions.yml:428`
- `.github/workflows/e2e-versions.yml:455`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tag or branch refs instead of immutable 40-character SHA commit hashes. Unpinned refs can be silently updated by the upstream repository, enabling supply-chain attacks. Failing references include: `actions/reusable-workflows/...@main` (branch ref) in basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml; `actions/checkout@v4` (tag ref) in e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, publish-immutable-actions.yml; `actions/publish-immutable-action@v0.0.4` (tag ref) in publish-immutable-actions.yml; `actions/publish-action@v0.3.0` (tag ref) in release-new-action-version.yml.

Locations:

- `.github/workflows/basic-validation.yml:17`
- `.github/workflows/check-dist.yml:16`
- `.github/workflows/codeql-analysis.yml:14`
- `.github/workflows/licensed.yml:14`
- `.github/workflows/update-config-files.yml:13`
- `.github/workflows/e2e-cache-dependency-path.yml:24`
- `.github/workflows/e2e-cache.yml:24`
- `.github/workflows/e2e-local-file.yml:23`
- `.github/workflows/e2e-publishing.yml:24`
- `.github/workflows/e2e-versions.yml:53`
- `.github/workflows/publish-immutable-actions.yml:14`
- `.github/workflows/publish-immutable-actions.yml:17`
- `.github/workflows/release-new-action-version.yml:22`

### missing-permissions (severity: medium)

Multiple workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows run with the default token permissions (which may be read/write depending on repository settings), violating the principle of least privilege. Affected files: basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml.

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

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings across 12 workflow files:

1. script-injection: Moved all ${{ steps.setup-java.outputs.path }} and ${{ matrix.version }} expressions out of run: shell strings into env: blocks (as SETUP_JAVA_PATH and MATRIX_VERSION respectively) in e2e-local-file.yml (3 locations) and e2e-versions.yml (13 locations).

2. unpinned-uses: Pinned all action references to full 40-char SHAs: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/reusable-workflows@main → @9e7901ee9359b9b3e7e990fcc499e43aa005eec3, actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978, actions/publish-action@v0.3.0 → @f784495ce78a41bac4ed7e34a73f0034015764bb. Original tags preserved as inline comments.

3. missing-permissions: Added top-level 'permissions: contents: read' to all 10 affected workflow files (basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml).

