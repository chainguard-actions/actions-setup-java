<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--setup-java/v5.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tag or branch refs instead of immutable 40-character SHA digests. This exposes the workflows to supply-chain attacks if the referenced tags are moved or the repositories are compromised. Failing references include: `actions/checkout@v7` (e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, zizmor.yml, publish-immutable-actions.yml), `actions/checkout@v6` (e2e-versions.yml), `actions/reusable-workflows/...@main` (basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml), `actions/publish-immutable-action@v0.0.4` (publish-immutable-actions.yml), `actions/publish-action@v0.4.0` (release-new-action-version.yml), `actions/setup-python@v6` (zizmor.yml), `github/codeql-action/upload-sarif@v4` (zizmor.yml).

Locations:

- `.github/workflows/basic-validation.yml:19`
- `.github/workflows/check-dist.yml:18`
- `.github/workflows/codeql-analysis.yml:17`
- `.github/workflows/e2e-cache-dependency-path.yml:25`
- `.github/workflows/e2e-cache.yml:25`
- `.github/workflows/e2e-local-file.yml:24`
- `.github/workflows/e2e-publishing.yml:26`
- `.github/workflows/e2e-versions.yml:75`
- `.github/workflows/licensed.yml:17`
- `.github/workflows/publish-immutable-actions.yml:16`
- `.github/workflows/release-new-action-version.yml:23`
- `.github/workflows/update-config-files.yml:15`
- `.github/workflows/zizmor.yml:24`

### script-injection (severity: high)

In the `setup-java-set-default` job of e2e-versions.yml, two `run:` blocks directly interpolate GitHub Actions expressions inside shell commands, violating rule (a). (1) The step 'Verify JAVA_HOME still points to Java 17' contains: `echo "Java 17 path=${{ steps.setup-java-17.outputs.path }}"` and `if [ "$JAVA_HOME" != "${{ steps.setup-java-17.outputs.path }}" ]` — the step output value is injected directly into the shell string before the shell parses it. (2) The step 'Verify Java 21 outputs are set' contains: `echo "Java 21 path=${{ steps.setup-java-21.outputs.path }}"`, `echo "Java 21 version=${{ steps.setup-java-21.outputs.version }}"`, and two `if [ -z "${{ steps.setup-java-21.outputs.* }}" ]` conditions. These should be routed through `env:` variables and then double-quoted in the shell script.

Locations:

- `.github/workflows/e2e-versions.yml:474`
- `.github/workflows/e2e-versions.yml:504`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all unpinned action references by replacing mutable tags/branches with immutable SHA digests: actions/checkout@v7 (→3d3c42e), actions/checkout@v6 (→d23441a), actions/reusable-workflows@main (→4735e71), actions/publish-immutable-action@v0.0.4 (→4bc8754), actions/publish-action@v0.4.0 (→23f4c6f), actions/setup-python@v6 (→ece7cb0), github/codeql-action/upload-sarif@v4 (→e064762). Fixed script injection in e2e-versions.yml setup-java-set-default job: moved ${{ steps.setup-java-17.outputs.path }} and ${{ steps.setup-java-21.outputs.* }} expressions into env: blocks and referenced them as plain shell variables ($JAVA_17_PATH, $JAVA_21_PATH, $JAVA_21_VERSION) in the run: scripts.

