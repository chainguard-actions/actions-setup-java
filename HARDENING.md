<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--setup-java/v5.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use mutable tag or branch refs instead of pinned SHA commits, making them vulnerable to supply-chain attacks. Affected refs include: actions/checkout@v7, actions/checkout@v6, actions/reusable-workflows/...@main, actions/publish-immutable-action@v0.0.4, actions/publish-action@v0.4.0, actions/setup-python@v6, github/codeql-action/upload-sarif@v4.

Locations:

- `.github/workflows/basic-validation.yml:20`
- `.github/workflows/check-dist.yml:20`
- `.github/workflows/codeql-analysis.yml:16`
- `.github/workflows/e2e-cache-dependency-path.yml:1`
- `.github/workflows/e2e-cache.yml:1`
- `.github/workflows/e2e-local-file.yml:1`
- `.github/workflows/e2e-publishing.yml:1`
- `.github/workflows/e2e-versions.yml:1`
- `.github/workflows/licensed.yml:16`
- `.github/workflows/publish-immutable-actions.yml:1`
- `.github/workflows/release-new-action-version.yml:1`
- `.github/workflows/update-config-files.yml:1`
- `.github/workflows/zizmor.yml:1`

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are directly interpolated inside run: shell command strings in e2e-versions.yml. In the 'setup-java-set-default' job, the step 'Verify JAVA_HOME still points to Java 17' contains: `echo "Java 17 path=${{ steps.setup-java-17.outputs.path }}"` and `if [ "$JAVA_HOME" != "${{ steps.setup-java-17.outputs.path }}" ]`. The step 'Verify Java 21 outputs are set' contains: `echo "Java 21 path=${{ steps.setup-java-21.outputs.path }}"`, `echo "Java 21 version=${{ steps.setup-java-21.outputs.version }}"`, `if [ -z "${{ steps.setup-java-21.outputs.path }}" ]`, and `if [ -z "${{ steps.setup-java-21.outputs.version }}" ]`. These ${{ }} expressions are expanded by the template engine before the shell sees them, allowing injection of shell metacharacters.

Locations:

- `.github/workflows/e2e-versions.yml:560`
- `.github/workflows/e2e-versions.yml:580`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references by replacing mutable tags/branches with full commit SHAs across all 13 workflow files: actions/checkout@v7 → SHA 9c091bb, actions/checkout@v6 → SHA df4cb1c, actions/reusable-workflows@main → SHA 4735e71, actions/publish-immutable-action@v0.0.4 → SHA 4bc8754, actions/publish-action@v0.4.0 → SHA 23f4c6f, actions/setup-python@v6 → SHA ece7cb0, github/codeql-action/upload-sarif@v4 → SHA 99df26d. Fixed script injection in e2e-versions.yml setup-java-set-default job by moving ${{ steps.setup-java-17.outputs.path }}, ${{ steps.setup-java-21.outputs.path }}, and ${{ steps.setup-java-21.outputs.version }} expressions out of run: shell strings and into env: blocks, referencing them as plain environment variables ($JAVA_17_PATH, $JAVA_21_PATH, $JAVA_21_VERSION) in the shell scripts. Also repaired zizmor.yml which was accidentally corrupted during editing.

