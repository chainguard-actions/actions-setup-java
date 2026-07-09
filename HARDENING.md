<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v4.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--setup-java/v4.8.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned SHA digests. Unpinned references are vulnerable to supply-chain attacks if the referenced tag or branch is updated with malicious code.

Failing references:
- basic-validation.yml: `uses: actions/reusable-workflows/.github/workflows/basic-validation.yml@main`
- check-dist.yml: `uses: actions/reusable-workflows/.github/workflows/check-dist.yml@main`
- codeql-analysis.yml: `uses: actions/reusable-workflows/.github/workflows/codeql-analysis.yml@main`
- licensed.yml: `uses: actions/reusable-workflows/.github/workflows/licensed.yml@main`
- update-config-files.yml: `uses: actions/reusable-workflows/.github/workflows/update-config-files.yml@main`
- publish-immutable-actions.yml: `uses: actions/checkout@v4`, `uses: actions/publish-immutable-action@v0.0.4`
- release-new-action-version.yml: `uses: actions/publish-action@v0.3.0`
- e2e-cache-dependency-path.yml: `uses: actions/checkout@v4`
- e2e-cache.yml: `uses: actions/checkout@v4`
- e2e-local-file.yml: `uses: actions/checkout@v4`
- e2e-publishing.yml: `uses: actions/checkout@v4`
- e2e-versions.yml: `uses: actions/checkout@v4`

Locations:

- `.github/workflows/basic-validation.yml:14`
- `.github/workflows/check-dist.yml:14`
- `.github/workflows/codeql-analysis.yml:12`
- `.github/workflows/licensed.yml:14`
- `.github/workflows/update-config-files.yml:12`
- `.github/workflows/publish-immutable-actions.yml:14`
- `.github/workflows/publish-immutable-actions.yml:16`
- `.github/workflows/release-new-action-version.yml:23`
- `.github/workflows/e2e-cache-dependency-path.yml:22`
- `.github/workflows/e2e-cache.yml:22`
- `.github/workflows/e2e-local-file.yml:22`
- `.github/workflows/e2e-publishing.yml:22`
- `.github/workflows/e2e-versions.yml:57`

### missing-permissions (severity: medium)

Multiple workflow files have no top-level `permissions:` key and no job-level `permissions:` keys. Without explicit permissions, workflows run with the default token permissions which may be overly broad (e.g., write access to contents). Affected files: basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, update-config-files.yml.

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

### script-injection (severity: high)

Multiple `run:` blocks directly interpolate GitHub Actions expressions (`${{ ... }}`) inside shell command strings. This is a script injection vulnerability (sub-rule a): the expression value is substituted by the Actions runner before the shell sees it, allowing an attacker who controls the value (e.g., via matrix inputs or step outputs) to inject arbitrary shell commands.

Affected patterns include:
- `run: bash __tests__/verify-java.sh "${{ matrix.version }}" "${{ steps.setup-java.outputs.path }}"`
- `run: bash __tests__/verify-java.sh "11.0.10" "${{ steps.setup-java.outputs.path }}"`

All occurrences should use env vars instead: set the value in an `env:` block and reference `"$ENV_VAR"` in the shell script.

Locations:

- `.github/workflows/e2e-versions.yml:68`
- `.github/workflows/e2e-versions.yml:107`
- `.github/workflows/e2e-versions.yml:131`
- `.github/workflows/e2e-versions.yml:163`
- `.github/workflows/e2e-versions.yml:183`
- `.github/workflows/e2e-versions.yml:200`
- `.github/workflows/e2e-versions.yml:217`
- `.github/workflows/e2e-versions.yml:285`
- `.github/workflows/e2e-versions.yml:310`
- `.github/workflows/e2e-versions.yml:334`
- `.github/workflows/e2e-versions.yml:360`
- `.github/workflows/e2e-versions.yml:385`
- `.github/workflows/e2e-versions.yml:410`
- `.github/workflows/e2e-local-file.yml:46`
- `.github/workflows/e2e-local-file.yml:73`
- `.github/workflows/e2e-local-file.yml:100`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three finding types across the workflow files: (1) unpinned-uses: pinned actions/checkout@v4 to SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/publish-immutable-action@v0.0.4 to SHA 4bc8754ffc40f27910afb20287dbbbb675a4e978, actions/publish-action@v0.3.0 to SHA f784495ce78a41bac4ed7e34a73f0034015764bb, and actions/reusable-workflows@main to SHA 4735e71081024a944852f4ab9d1495b6dd2de8f2 in all affected files; (2) missing-permissions: added 'permissions: {}' top-level block to basic-validation.yml, check-dist.yml, codeql-analysis.yml, e2e-cache-dependency-path.yml, e2e-cache.yml, e2e-local-file.yml, e2e-publishing.yml, e2e-versions.yml, licensed.yml, and update-config-files.yml; (3) script-injection: moved all ${{ steps.setup-java.outputs.path }} and ${{ matrix.version }} expressions from run: shell strings into env: blocks in e2e-versions.yml (13 occurrences) and e2e-local-file.yml (3 occurrences), referencing them as $SETUP_JAVA_PATH and $MATRIX_VERSION environment variables in the shell scripts.

