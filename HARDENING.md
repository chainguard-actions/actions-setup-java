<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v4.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v4.8.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple `run:` blocks directly interpolate GitHub Actions expressions inside shell command strings (rule a). In e2e-local-file.yml, `${{ steps.setup-java.outputs.path }}` is interpolated directly in `run: bash __tests__/verify-java.sh "11.0.10" "${{ steps.setup-java.outputs.path }}"`. In e2e-versions.yml, both `${{ matrix.version }}` and `${{ steps.setup-java.outputs.path }}` are interpolated directly in `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"`. The `matrix.*` and `steps.*.outputs.*` contexts are workflow-controllable and flow through YAML template substitution before the shell processes them, enabling script injection.

Locations:

- `.github/workflows/e2e-local-file.yml:44`
- `.github/workflows/e2e-local-file.yml:72`
- `.github/workflows/e2e-local-file.yml:100`
- `.github/workflows/e2e-versions.yml:67`
- `.github/workflows/e2e-versions.yml:100`
- `.github/workflows/e2e-versions.yml:122`
- `.github/workflows/e2e-versions.yml:152`
- `.github/workflows/e2e-versions.yml:172`
- `.github/workflows/e2e-versions.yml:191`
- `.github/workflows/e2e-versions.yml:210`
- `.github/workflows/e2e-versions.yml:280`
- `.github/workflows/e2e-versions.yml:303`
- `.github/workflows/e2e-versions.yml:330`
- `.github/workflows/e2e-versions.yml:357`
- `.github/workflows/e2e-versions.yml:384`
- `.github/workflows/e2e-versions.yml:411`

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned 40-character SHA commit hashes, making them vulnerable to supply-chain attacks. Failing references include: `actions/reusable-workflows/.github/workflows/basic-validation.yml@main`, `actions/reusable-workflows/.github/workflows/check-dist.yml@main`, `actions/reusable-workflows/.github/workflows/codeql-analysis.yml@main`, `actions/reusable-workflows/.github/workflows/licensed.yml@main`, `actions/reusable-workflows/.github/workflows/update-config-files.yml@main`, `actions/checkout@v4` (in e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, publish-immutable-actions.yml), `actions/publish-immutable-action@v0.0.4`, and `actions/publish-action@v0.3.0`.

Locations:

- `.github/workflows/basic-validation.yml:16`
- `.github/workflows/check-dist.yml:15`
- `.github/workflows/codeql-analysis.yml:13`
- `.github/workflows/licensed.yml:14`
- `.github/workflows/update-config-files.yml:11`
- `.github/workflows/publish-immutable-actions.yml:14`
- `.github/workflows/publish-immutable-actions.yml:17`
- `.github/workflows/release-new-action-version.yml:22`
- `.github/workflows/e2e-cache-dependency-path.yml:22`
- `.github/workflows/e2e-cache.yml:22`
- `.github/workflows/e2e-local-file.yml:22`
- `.github/workflows/e2e-publishing.yml:22`
- `.github/workflows/e2e-versions.yml:62`

### missing-permissions (severity: medium)

Multiple workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows inherit the default repository token permissions (which may be broad). Affected files: basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml.

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

1. script-injection: Moved all ${{ steps.setup-java.outputs.path }} and ${{ matrix.version }} expressions from run: shell strings into step env: blocks (as JAVA_PATH and MATRIX_VERSION), then referenced them as plain shell variables in the run: commands. Affected: e2e-local-file.yml (3 steps) and e2e-versions.yml (13 steps).

2. unpinned-uses: Pinned all mutable action references to full 40-char SHAs:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 (all e2e-* workflows)
   - actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978
   - actions/publish-action@v0.3.0 → @f784495ce78a41bac4ed7e34a73f0034015764bb
   - actions/reusable-workflows@main → @4735e71081024a944852f4ab9d1495b6dd2de8f2 (all 5 reusable workflow calls)

3. missing-permissions: Added 'permissions: {}' top-level block to all 10 affected workflow files (basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml).

