<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v5.2.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use mutable tag/branch refs instead of pinned 40-character SHA commit hashes, making them vulnerable to supply-chain attacks. Failing references include: `actions/checkout@v6`, `actions/reusable-workflows/...@main`, `actions/publish-immutable-action@v0.0.4`, `actions/publish-action@v0.4.0`.

Locations:

- `.github/workflows/basic-validation.yml:12`
- `.github/workflows/check-dist.yml:13`
- `.github/workflows/codeql-analysis.yml:12`
- `.github/workflows/e2e-cache-dependency-path.yml:24`
- `.github/workflows/e2e-cache.yml:24`
- `.github/workflows/e2e-local-file.yml:23`
- `.github/workflows/e2e-publishing.yml:24`
- `.github/workflows/e2e-versions.yml:68`
- `.github/workflows/licensed.yml:13`
- `.github/workflows/publish-immutable-actions.yml:18`
- `.github/workflows/release-new-action-version.yml:24`
- `.github/workflows/update-config-files.yml:11`

### script-injection (severity: high)

Multiple `run:` blocks directly interpolate GitHub Actions expressions (`${{ ... }}`) inside shell command strings (sub-rule a). This includes `${{ steps.setup-java.outputs.path }}` and `${{ matrix.version }}` embedded directly in bash command arguments. Even though these specific values may not be directly attacker-controlled, any `${{ ... }}` inside a `run:` block flows through YAML template substitution before the shell processes it, creating a script injection risk. Examples: `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"`

Locations:

- `.github/workflows/e2e-local-file.yml:44`
- `.github/workflows/e2e-local-file.yml:73`
- `.github/workflows/e2e-local-file.yml:102`
- `.github/workflows/e2e-versions.yml:79`
- `.github/workflows/e2e-versions.yml:116`
- `.github/workflows/e2e-versions.yml:148`
- `.github/workflows/e2e-versions.yml:183`
- `.github/workflows/e2e-versions.yml:205`
- `.github/workflows/e2e-versions.yml:227`
- `.github/workflows/e2e-versions.yml:249`
- `.github/workflows/e2e-versions.yml:335`
- `.github/workflows/e2e-versions.yml:362`
- `.github/workflows/e2e-versions.yml:400`
- `.github/workflows/e2e-versions.yml:432`
- `.github/workflows/e2e-versions.yml:464`
- `.github/workflows/e2e-versions.yml:497`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Each file should declare minimal required permissions.

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

Fixed all three finding types across 12 workflow files:

1. **unpinned-uses**: Pinned all mutable action refs to full 40-char SHAs:
   - actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10 # v6
   - actions/reusable-workflows/...@main → @4735e71081024a944852f4ab9d1495b6dd2de8f2 # main
   - actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 # v0.0.4
   - actions/publish-action@v0.4.0 → @23f4c6f12633a2da8f44938b71fde9afec138fb4 # v0.4.0

2. **script-injection**: Moved all ${{ matrix.version }} and ${{ steps.setup-java.outputs.path }} expressions out of run: shell strings into step env: blocks, referenced as $MATRIX_VERSION and $SETUP_JAVA_PATH respectively. Fixed in e2e-local-file.yml (3 locations) and e2e-versions.yml (13 locations).

3. **missing-permissions**: Added top-level `permissions: contents: read` to 10 workflow files that lacked any permissions declaration. publish-immutable-actions.yml already had job-level permissions; release-new-action-version.yml already had top-level permissions: contents: write.

