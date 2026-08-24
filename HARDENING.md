<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v6.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v6.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple run: blocks in benchmark-cache-restore.yml directly interpolate ${{ matrix.tool }}, ${{ matrix.profile }}, and ${{ matrix.os }} expressions (rule a: direct expression interpolation). While matrix values are defined in the workflow, matrix.* is a workflow-controllable context and any ${{ ... }} inside a run: shell command is a script-injection risk. Affected steps include: 'Prepare benchmark inputs' (both jobs), 'Populate benchmark caches', 'Reset/Start/Record' steps for all baseline and candidate iterations, and 'Summarize benchmark'. Example offending lines: `run: bash __tests__/benchmark-cache-restore.sh prepare "${{ matrix.tool }}" "${{ matrix.profile }}"` and `run: bash __tests__/benchmark-cache-restore.sh record "${{ matrix.tool }}" "${{ matrix.os }}" "${{ matrix.profile }}" baseline 1 "$CACHE_HIT"`

Locations:

- `.github/workflows/benchmark-cache-restore.yml:44`
- `.github/workflows/benchmark-cache-restore.yml:57`
- `.github/workflows/benchmark-cache-restore.yml:80`
- `.github/workflows/benchmark-cache-restore.yml:83`
- `.github/workflows/benchmark-cache-restore.yml:85`
- `.github/workflows/benchmark-cache-restore.yml:96`
- `.github/workflows/benchmark-cache-restore.yml:99`
- `.github/workflows/benchmark-cache-restore.yml:101`
- `.github/workflows/benchmark-cache-restore.yml:112`
- `.github/workflows/benchmark-cache-restore.yml:115`
- `.github/workflows/benchmark-cache-restore.yml:117`
- `.github/workflows/benchmark-cache-restore.yml:128`
- `.github/workflows/benchmark-cache-restore.yml:131`
- `.github/workflows/benchmark-cache-restore.yml:133`
- `.github/workflows/benchmark-cache-restore.yml:144`
- `.github/workflows/benchmark-cache-restore.yml:147`
- `.github/workflows/benchmark-cache-restore.yml:149`
- `.github/workflows/benchmark-cache-restore.yml:160`
- `.github/workflows/benchmark-cache-restore.yml:163`
- `.github/workflows/benchmark-cache-restore.yml:165`
- `.github/workflows/benchmark-cache-restore.yml:176`
- `.github/workflows/benchmark-cache-restore.yml:179`

### unpinned-uses (severity: high)

All workflow files use mutable tag-based or branch-based refs instead of pinned 40-character SHA commit hashes. Unpinned references are vulnerable to supply-chain attacks if the referenced action or reusable workflow is compromised or its tag is moved. Failing references include: actions/checkout@v7, actions/upload-artifact@v7, actions/setup-python@v7, github/codeql-action/upload-sarif@v4, actions/publish-action@v0.4.0, actions/publish-immutable-action@v0.0.4, actions/reusable-workflows/.github/workflows/basic-validation.yml@main, actions/reusable-workflows/.github/workflows/check-dist.yml@main, actions/reusable-workflows/.github/workflows/codeql-analysis.yml@main, actions/reusable-workflows/.github/workflows/licensed.yml@main, actions/reusable-workflows/.github/workflows/update-config-files.yml@main.

Locations:

- `.github/workflows/basic-validation.yml:17`
- `.github/workflows/benchmark-cache-restore.yml:35`
- `.github/workflows/benchmark-cache-restore.yml:39`
- `.github/workflows/benchmark-cache-restore.yml:43`
- `.github/workflows/benchmark-cache-restore.yml:185`
- `.github/workflows/check-dist.yml:17`
- `.github/workflows/codeql-analysis.yml:17`
- `.github/workflows/e2e-cache.yml:22`
- `.github/workflows/e2e-local-file.yml:22`
- `.github/workflows/e2e-publishing.yml:22`
- `.github/workflows/e2e-smoke.yml:52`
- `.github/workflows/e2e-versions.yml:82`
- `.github/workflows/licensed.yml:17`
- `.github/workflows/publish-immutable-actions.yml:18`
- `.github/workflows/publish-immutable-actions.yml:22`
- `.github/workflows/release-new-action-version.yml:22`
- `.github/workflows/update-config-files.yml:14`
- `.github/workflows/zizmor.yml:27`
- `.github/workflows/zizmor.yml:31`
- `.github/workflows/zizmor.yml:50`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in benchmark-cache-restore.yml by moving all ${{ matrix.tool }}, ${{ matrix.profile }}, and ${{ matrix.os }} expressions from run: shell strings into env: blocks (as MATRIX_TOOL, MATRIX_PROFILE, MATRIX_OS), then referencing them as plain env vars. Fixed unpinned-uses across all 11 affected workflow files by pinning: actions/checkout@v7→SHA 3d3c42e, actions/upload-artifact@v7→SHA 043fb46, actions/setup-python@v7→SHA 5fda3b9, github/codeql-action/upload-sarif@v4→SHA db488dd, actions/publish-action@v0.4.0→SHA 23f4c6f, actions/publish-immutable-action@v0.0.4→SHA 4bc8754, and actions/reusable-workflows@main→SHA d468c63. All SHAs were resolved via lookup_action_sha.

