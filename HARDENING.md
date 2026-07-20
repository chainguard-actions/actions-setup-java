<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v5.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable tags or branch names instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks. Failing references include: `actions/checkout@v6`, `actions/publish-immutable-action@v0.0.4`, `actions/publish-action@v0.4.0`, and `actions/reusable-workflows/...@main` across all workflow files.

Locations:

- `.github/workflows/basic-validation.yml:13`
- `.github/workflows/check-dist.yml:14`
- `.github/workflows/codeql-analysis.yml:12`
- `.github/workflows/licensed.yml:13`
- `.github/workflows/update-config-files.yml:10`
- `.github/workflows/e2e-cache-dependency-path.yml:24`
- `.github/workflows/e2e-cache.yml:30`
- `.github/workflows/e2e-local-file.yml:24`
- `.github/workflows/e2e-publishing.yml:26`
- `.github/workflows/e2e-versions.yml:66`
- `.github/workflows/publish-immutable-actions.yml:18`
- `.github/workflows/publish-immutable-actions.yml:21`
- `.github/workflows/release-new-action-version.yml:26`

### script-injection (severity: high)

Multiple `run:` blocks directly interpolate GitHub Actions expressions into shell command strings (sub-rule a). In `e2e-versions.yml`, `${{ matrix.version }}` and `${{ steps.setup-java.outputs.path }}` are interpolated directly into `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"` across 14 steps. In `e2e-local-file.yml`, `${{ steps.setup-java.outputs.path }}` is interpolated directly into `run: bash __tests__/verify-java.sh ... "${{ steps.setup-java.outputs.path }}"` across 3 steps. These expressions are expanded by the YAML template engine before the shell sees them, allowing injection of shell metacharacters.

Locations:

- `.github/workflows/e2e-versions.yml:76`
- `.github/workflows/e2e-versions.yml:97`
- `.github/workflows/e2e-versions.yml:131`
- `.github/workflows/e2e-versions.yml:157`
- `.github/workflows/e2e-versions.yml:193`
- `.github/workflows/e2e-versions.yml:215`
- `.github/workflows/e2e-versions.yml:234`
- `.github/workflows/e2e-versions.yml:253`
- `.github/workflows/e2e-versions.yml:313`
- `.github/workflows/e2e-versions.yml:337`
- `.github/workflows/e2e-versions.yml:360`
- `.github/workflows/e2e-versions.yml:390`
- `.github/workflows/e2e-versions.yml:416`
- `.github/workflows/e2e-versions.yml:447`
- `.github/workflows/e2e-local-file.yml:49`
- `.github/workflows/e2e-local-file.yml:79`
- `.github/workflows/e2e-local-file.yml:109`

### missing-permissions (severity: medium)

Ten workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows inherit the default (potentially broad) repository permissions. Affected files: basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml.

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

Fixed all 13 unpinned action references by resolving full SHA digests via lookup_action_sha: actions/checkout@v6→df4cb1c, actions/publish-immutable-action@v0.0.4→4bc8754, actions/publish-action@v0.4.0→23f4c6f, actions/reusable-workflows@main→4735e71. Fixed all 17 script-injection instances in e2e-versions.yml (14 locations) and e2e-local-file.yml (3 locations) by moving ${{ matrix.version }} and ${{ steps.setup-java.outputs.path }} into step env: blocks as MATRIX_VERSION and SETUP_JAVA_PATH, then referencing them as plain shell variables in run: scripts. Added top-level 'permissions: contents: read' to all 10 workflow files that lacked permissions blocks (basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml).

