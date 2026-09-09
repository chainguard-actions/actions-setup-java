<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v6.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v6.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Every `uses:` reference across all workflow files is pinned to a mutable tag or branch instead of a full 40-character commit SHA. This exposes the action to supply-chain attacks where a tag is silently moved to a malicious commit. Failing references include: `actions/checkout@v7`, `actions/reusable-workflows/...@main`, `actions/reusable-workflows/.../codeql-analysis.yml@main`, `actions/reusable-workflows/.../licensed.yml@main`, `actions/upload-artifact@v7`, `actions/checkout@v7`, `actions/publish-immutable-action@v0.0.4`, `actions/publish-action@v0.4.0`, `actions/setup-python@v7`, `github/codeql-action/upload-sarif@v4`, and `actions/reusable-workflows/.../update-config-files.yml@main`.

Locations:

- `.github/workflows/basic-validation.yml:19`
- `.github/workflows/benchmark-cache-restore.yml:36`
- `.github/workflows/benchmark-cache-restore.yml:40`
- `.github/workflows/benchmark-cache-restore.yml:46`
- `.github/workflows/benchmark-cache-restore.yml:163`
- `.github/workflows/check-dist.yml:17`
- `.github/workflows/codeql-analysis.yml:16`
- `.github/workflows/e2e-cache.yml:27`
- `.github/workflows/e2e-local-file.yml:27`
- `.github/workflows/e2e-publishing.yml:28`
- `.github/workflows/e2e-smoke.yml:55`
- `.github/workflows/e2e-versions.yml:72`
- `.github/workflows/licensed.yml:16`
- `.github/workflows/publish-immutable-actions.yml:16`
- `.github/workflows/publish-immutable-actions.yml:19`
- `.github/workflows/release-new-action-version.yml:22`
- `.github/workflows/update-config-files.yml:16`
- `.github/workflows/zizmor.yml:24`
- `.github/workflows/zizmor.yml:28`
- `.github/workflows/zizmor.yml:40`

### script-injection (severity: high)

Multiple `run:` steps in benchmark-cache-restore.yml directly interpolate GitHub Actions expressions into shell command strings (sub-rule a). `${{ matrix.tool }}`, `${{ matrix.profile }}`, `${{ matrix.os }}`, and `${{ steps.*.outputs.cache-hit }}` are substituted by the Actions template engine before the shell parses the command, allowing an attacker who controls matrix values or step outputs to inject arbitrary shell commands. Offending lines include:
- `run: bash __tests__/benchmark-cache-restore.sh prepare "${{ matrix.tool }}" "${{ matrix.profile }}"`
- `run: bash __tests__/benchmark-cache-restore.sh reset "${{ matrix.tool }}"`
- `run: bash __tests__/benchmark-cache-restore.sh start "${{ matrix.tool }}"`
- `run: bash __tests__/benchmark-cache-restore.sh record "${{ matrix.tool }}" "${{ matrix.os }}" "${{ matrix.profile }}" ...`
- `run: bash __tests__/benchmark-cache-restore.sh summarize "${{ matrix.tool }}" "$GITHUB_STEP_SUMMARY"`
Fix: move matrix values into `env:` variables and reference them as quoted shell variables (e.g. `"$MATRIX_TOOL"`).

Locations:

- `.github/workflows/benchmark-cache-restore.yml:52`
- `.github/workflows/benchmark-cache-restore.yml:56`
- `.github/workflows/benchmark-cache-restore.yml:57`
- `.github/workflows/benchmark-cache-restore.yml:76`
- `.github/workflows/benchmark-cache-restore.yml:79`
- `.github/workflows/benchmark-cache-restore.yml:81`
- `.github/workflows/benchmark-cache-restore.yml:89`
- `.github/workflows/benchmark-cache-restore.yml:93`
- `.github/workflows/benchmark-cache-restore.yml:95`
- `.github/workflows/benchmark-cache-restore.yml:103`
- `.github/workflows/benchmark-cache-restore.yml:107`
- `.github/workflows/benchmark-cache-restore.yml:109`
- `.github/workflows/benchmark-cache-restore.yml:117`
- `.github/workflows/benchmark-cache-restore.yml:121`
- `.github/workflows/benchmark-cache-restore.yml:123`
- `.github/workflows/benchmark-cache-restore.yml:131`
- `.github/workflows/benchmark-cache-restore.yml:135`
- `.github/workflows/benchmark-cache-restore.yml:137`
- `.github/workflows/benchmark-cache-restore.yml:145`
- `.github/workflows/benchmark-cache-restore.yml:149`
- `.github/workflows/benchmark-cache-restore.yml:157`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned `uses:` references across 10 workflow files by resolving each tag/branch to its full 40-character commit SHA using lookup_action_sha. Fixed script injection in benchmark-cache-restore.yml by moving all ${{ matrix.tool }}, ${{ matrix.profile }}, ${{ matrix.os }}, and ${{ steps.*.outputs.cache-hit }} expressions from run: shell strings into env: blocks, referencing them as $MATRIX_TOOL, $MATRIX_PROFILE, $MATRIX_OS, and $CACHE_HIT respectively. All SHAs were resolved via lookup_action_sha and are accurate.

