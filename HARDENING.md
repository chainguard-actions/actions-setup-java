<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--setup-java/v5.2.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are directly interpolated into run: shell commands. In e2e-local-file.yml, `${{ steps.setup-java.outputs.path }}` is interpolated directly into bash command strings (e.g., `run: bash __tests__/verify-java.sh "11.0.10" "${{ steps.setup-java.outputs.path }}"`). In e2e-versions.yml, both `${{ matrix.version }}` and `${{ steps.setup-java.outputs.path }}` are interpolated directly into bash commands across 13 steps. These values flow through YAML template substitution before the shell sees them, enabling script injection.

Locations:

- `.github/workflows/e2e-local-file.yml:44`
- `.github/workflows/e2e-local-file.yml:75`
- `.github/workflows/e2e-local-file.yml:106`
- `.github/workflows/e2e-versions.yml:79`
- `.github/workflows/e2e-versions.yml:113`
- `.github/workflows/e2e-versions.yml:143`
- `.github/workflows/e2e-versions.yml:185`
- `.github/workflows/e2e-versions.yml:207`
- `.github/workflows/e2e-versions.yml:228`
- `.github/workflows/e2e-versions.yml:250`
- `.github/workflows/e2e-versions.yml:330`
- `.github/workflows/e2e-versions.yml:358`
- `.github/workflows/e2e-versions.yml:393`
- `.github/workflows/e2e-versions.yml:424`
- `.github/workflows/e2e-versions.yml:455`
- `.github/workflows/e2e-versions.yml:490`

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags instead of pinned SHA hashes. Failing references include: `actions/reusable-workflows/.github/workflows/basic-validation.yml@main` (basic-validation.yml), `actions/reusable-workflows/.github/workflows/check-dist.yml@main` (check-dist.yml), `actions/reusable-workflows/.github/workflows/codeql-analysis.yml@main` (codeql-analysis.yml), `actions/checkout@v6` (e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml), `actions/reusable-workflows/.github/workflows/licensed.yml@main` (licensed.yml), `actions/checkout@v6` and `actions/publish-immutable-action@v0.0.4` (publish-immutable-actions.yml), `actions/publish-action@v0.4.0` (release-new-action-version.yml), `actions/reusable-workflows/.github/workflows/update-config-files.yml@main` (update-config-files.yml). None use 40-character hex SHA refs.

Locations:

- `.github/workflows/basic-validation.yml:14`
- `.github/workflows/check-dist.yml:14`
- `.github/workflows/codeql-analysis.yml:13`
- `.github/workflows/e2e-cache-dependency-path.yml:22`
- `.github/workflows/e2e-cache.yml:22`
- `.github/workflows/e2e-local-file.yml:23`
- `.github/workflows/e2e-publishing.yml:22`
- `.github/workflows/e2e-versions.yml:63`
- `.github/workflows/licensed.yml:13`
- `.github/workflows/publish-immutable-actions.yml:14`
- `.github/workflows/release-new-action-version.yml:20`
- `.github/workflows/update-config-files.yml:12`

### missing-permissions (severity: medium)

The following workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any job, meaning they run with the default (potentially broad) token permissions: basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml.

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

1. script-injection: Moved all ${{ steps.setup-java.outputs.path }} and ${{ matrix.version }} expressions out of run: shell commands into env: blocks in e2e-local-file.yml (3 Verify Java steps) and e2e-versions.yml (13 Verify Java steps). Shell scripts now reference $SETUP_JAVA_PATH and $MATRIX_VERSION as plain environment variables.

2. unpinned-uses: Pinned all mutable references to full SHA hashes:
   - actions/reusable-workflows@main → @09976383aa8780d306ee271bd21bb77a54fad474 # main
   - actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10 # v6
   - actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 # v0.0.4
   - actions/publish-action@v0.4.0 → @23f4c6f12633a2da8f44938b71fde9afec138fb4 # v0.4.0

3. missing-permissions: Added top-level 'permissions: {}' to basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, and update-config-files.yml.

