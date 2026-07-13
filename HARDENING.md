<!-- markdownlint-disable -->

# Hardening Report: actions--setup-java/v5.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **actions--setup-java/v5.5.0** was hardened automatically. 0 finding(s) were identified and resolved across 2 iteration(s).

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all unpinned action references to full commit SHAs across 13 workflow files:
- actions/reusable-workflows@main → @4735e71081024a944852f4ab9d1495b6dd2de8f2 in basic-validation.yml, check-dist.yml, codeql-analysis.yml, licensed.yml, update-config-files.yml
- actions/checkout@v7 → @9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 in e2e-cache-dependency-path.yml (3x), e2e-cache.yml (6x), e2e-local-file.yml (3x), e2e-publishing.yml (4x), e2e-versions.yml (16x), publish-immutable-actions.yml, zizmor.yml
- actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10 in e2e-versions.yml (setup-java-set-default job)
- actions/publish-immutable-action@v0.0.4 → @4bc8754ffc40f27910afb20287dbbbb675a4e978 in publish-immutable-actions.yml
- actions/publish-action@v0.4.0 → @23f4c6f12633a2da8f44938b71fde9afec138fb4 in release-new-action-version.yml
- actions/setup-python@v6 → @ece7cb06caefa5fff74198d8649806c4678c61a1 in zizmor.yml
- github/codeql-action/upload-sarif@v4 → @99df26d4f13ea111d4ec1a7dddef6063f76b97e9 in zizmor.yml
All SHAs were resolved using lookup_action_sha. Original tag names preserved as comments.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in the setup-java-set-default job in .github/workflows/e2e-versions.yml. Moved all ${{ steps.setup-java-17.outputs.path }}, ${{ steps.setup-java-21.outputs.path }}, and ${{ steps.setup-java-21.outputs.version }} expressions out of run: shell strings and into env: blocks (as JAVA_17_PATH, JAVA_21_PATH, and JAVA_21_VERSION respectively). The shell scripts now reference these as plain environment variables, preventing command injection via shell metacharacters in step outputs.

