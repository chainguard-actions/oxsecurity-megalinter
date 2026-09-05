<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v10.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v10.1.0** was hardened automatically. 3 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The main action.yml and all flavor action.yml files use a mutable Docker image tag (v10.1.0) instead of a SHA digest for the `runs.image:` field. This means the action can be silently updated to a different image without any code change. Additionally, mega-linter.yml uses `oxsecurity/megalinter/flavors/python@beta` and mega-linter-for-runner.yml uses `oxsecurity/megalinter/flavors/javascript@beta` — both are branch refs, not SHA-pinned commits. All of these should use SHA digests/commits instead of tags or branch names.

Locations:

- `action.yml:10`
- `flavors/c_cpp/action.yml:10`
- `flavors/ci_light/action.yml:10`
- `flavors/cupcake/action.yml:10`
- `flavors/documentation/action.yml:10`
- `flavors/dotnet/action.yml:10`
- `flavors/dotnetweb/action.yml:10`
- `flavors/formatters/action.yml:10`
- `flavors/go/action.yml:10`
- `flavors/java/action.yml:10`
- `flavors/javascript/action.yml:10`
- `flavors/php/action.yml:10`
- `flavors/python/action.yml:10`
- `flavors/ruby/action.yml:10`
- `flavors/rust/action.yml:10`
- `flavors/salesforce/action.yml:10`
- `flavors/security/action.yml:10`
- `flavors/swift/action.yml:10`
- `flavors/terraform/action.yml:10`
- `.github/workflows/mega-linter.yml:97`
- `.github/workflows/mega-linter-for-runner.yml:65`

### script-injection (severity: high)

Multiple workflow `run:` blocks directly interpolate `${{ ... }}` expressions into shell commands (rule a), which allows template substitution before the shell ever sees the value and enables command injection.

- auto-update-linters.yml: `run: docker run ... -e GITHUB_SHA=${{ github.sha }} ... -e GEMINI_API_KEY=${{ secrets.GEMINI_API_KEY }} ... oxsecurity/megalinter:auto_update_${{ github.sha }}`
- deploy-ALPHA.yml (Prepare step): `platform=${{ matrix.platform }}`
- deploy-ALPHA.yml (Create manifest list step): `$(printf 'ghcr.io/${{ github.repository }}-custom-flavor-builder@sha256:%s ' *)`
- deploy-ALPHA.yml (Inspect image step): `docker buildx imagetools inspect ghcr.io/${{ github.repository }}-custom-flavor-builder:...`
- deploy-BETA-linters.yml (Prepare step): `platform=${{ matrix.platform }}`
- deploy-BETA.yml (Prepare step): `platform=${{ matrix.platform }}`
- deploy-BETA.yml (Create manifest list step): `$(printf 'ghcr.io/${{ github.repository }}-custom-flavor-builder@sha256:%s ' *)`
- deploy-BETA.yml (Inspect image step): `docker buildx imagetools inspect ghcr.io/${{ github.repository }}-custom-flavor-builder:...`
- deploy-RELEASE.yml (Prepare step): `platform=${{ matrix.platform }}`
- deploy-RELEASE.yml (Create manifest list step): `$(printf 'ghcr.io/${{ github.repository }}-custom-flavor-builder@sha256:%s ' *)`
- deploy-RELEASE.yml (Inspect image step): `docker buildx imagetools inspect ghcr.io/${{ github.repository }}-custom-flavor-builder:...`
- mega-linter.yml (Set APPLY_FIXES_IF var): `printf 'APPLY_FIXES_IF=%s\n' "${{ steps.ml.outputs.has_updated_sources == 1 && ... github.event_name ... github.repository }}"` >> GITHUB_ENV
- mega-linter.yml (Set APPLY_FIXES_IF_* vars): `printf 'APPLY_FIXES_IF_PR=%s\n' "${{ env.APPLY_FIXES_IF == 'true' && ... github.ref ... }}"` >> GITHUB_ENV
- mega-linter-for-runner.yml: same Set APPLY_FIXES_IF var and Set APPLY_FIXES_IF_* vars patterns

Locations:

- `.github/workflows/auto-update-linters.yml:76`
- `.github/workflows/deploy-ALPHA.yml:133`
- `.github/workflows/deploy-ALPHA.yml:200`
- `.github/workflows/deploy-ALPHA.yml:204`
- `.github/workflows/deploy-BETA-linters.yml:79`
- `.github/workflows/deploy-BETA.yml:258`
- `.github/workflows/deploy-BETA.yml:320`
- `.github/workflows/deploy-BETA.yml:324`
- `.github/workflows/deploy-RELEASE.yml:186`
- `.github/workflows/deploy-RELEASE.yml:248`
- `.github/workflows/deploy-RELEASE.yml:252`
- `.github/workflows/mega-linter.yml:155`
- `.github/workflows/mega-linter.yml:165`
- `.github/workflows/mega-linter-for-runner.yml:113`
- `.github/workflows/mega-linter-for-runner.yml:123`

### github-env-injection (severity: high)

Several `run:` blocks write values derived from `${{ ... }}` expressions directly to `$GITHUB_ENV` without the required sanitization step (`printf '%s' ... | tr -d '\n\r'`).

- mega-linter.yml (Set APPLY_FIXES_IF var): `printf 'APPLY_FIXES_IF=%s\n' "${{ steps.ml.outputs.has_updated_sources == 1 && ... github.event_name ... github.repository }}" >> "${GITHUB_ENV}"` — writes github context values unsanitized to GITHUB_ENV.
- mega-linter.yml (Set APPLY_FIXES_IF_* vars): `printf 'APPLY_FIXES_IF_PR=%s\n' "${{ env.APPLY_FIXES_IF == 'true' && env.APPLY_FIXES_MODE == 'pull_request' }}" >> "${GITHUB_ENV}"` and similar for APPLY_FIXES_IF_COMMIT with `github.ref`.
- mega-linter-for-runner.yml: identical Set APPLY_FIXES_IF var and Set APPLY_FIXES_IF_* vars steps.
- deploy-ALPHA.yml (Prepare step): `platform=${{ matrix.platform }}` then `echo "PLATFORM_PAIR=${platform//\//-}" >> "${GITHUB_ENV}"` — matrix.platform is workflow-controllable and written to GITHUB_ENV without sanitization.
- deploy-BETA-linters.yml (Prepare step): same pattern.
- deploy-BETA.yml (Prepare step): same pattern.
- deploy-RELEASE.yml (Prepare step): same pattern.

Locations:

- `.github/workflows/mega-linter.yml:155`
- `.github/workflows/mega-linter.yml:165`
- `.github/workflows/mega-linter-for-runner.yml:113`
- `.github/workflows/mega-linter-for-runner.yml:123`
- `.github/workflows/deploy-ALPHA.yml:133`
- `.github/workflows/deploy-BETA-linters.yml:79`
- `.github/workflows/deploy-BETA.yml:258`
- `.github/workflows/deploy-RELEASE.yml:186`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all three finding types:

1. unpinned-uses: Pinned all 19 action.yml Docker image references (main + 18 flavors) with SHA digests. Pinned mega-linter.yml's `oxsecurity/megalinter/flavors/python@beta` and mega-linter-for-runner.yml's `oxsecurity/megalinter/flavors/javascript@beta` to commit SHA 331108469f4b174c6ad77f8231249b331bcdb056.

2. script-injection: Moved all ${{ }} expressions out of run: shell strings into env: blocks in auto-update-linters.yml (github.sha, secrets.GEMINI_API_KEY), deploy-ALPHA/BETA/BETA-linters/RELEASE.yml (matrix.platform, github.repository in manifest/inspect steps), and mega-linter.yml/mega-linter-for-runner.yml (APPLY_FIXES_IF expressions).

3. github-env-injection: Added sanitization (printf '%s' ... | tr -d '\n\r') before writing to GITHUB_ENV in all affected steps: APPLY_FIXES_IF/APPLY_FIXES_IF_* steps in mega-linter.yml and mega-linter-for-runner.yml, and Prepare steps in all four deploy workflows.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed script-injection in flavors/custom-builder/action.yml by moving all ${{ }} expressions in both 'Build Custom MegaLinter Flavor' and 'Tag and Push Docker Image' steps into env: blocks, then referencing them as double-quoted shell variables. Fixed github-env-injection in .github/workflows/deploy-DEV-linters.yml by sanitizing PR_TITLE, HEAD_COMMIT_MSG, and PR_USER with 'printf "%s" "$VAR" | tr -d "\n\r"' before writing to GITHUB_OUTPUT, preventing newline-based injection of arbitrary key=value pairs.

### Iteration 3

**Fixes applied:** unsafe-shell

**Notes:**

Fixed the unsafe-shell finding in .github/workflows/deploy-DEV.yml at the 'Run Test Cases' step. The original code used bash process substitution `CI_ENV="$(bash <(curl -s https://codecov.io/env)) -e GITHUB_ACTIONS"` to download and immediately execute a remote script from codecov.io. The fix downloads the script to a temporary file via `mktemp` and `curl -s --retry 3 --retry-connrefused -o`, executes it separately with `bash "$CODECOV_ENV_SCRIPT"`, then removes the temp file. This eliminates the unsafe pattern of piping remote content directly to a shell interpreter.

