<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v9.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v9.6.0** was hardened automatically. 9 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): `${{ github.sha }}` and `${{ secrets.GEMINI_API_KEY }}` are directly interpolated inside a `run:` shell command string in the 'Collect latest versions and help' step. The line reads: `run: docker run -e GITHUB_SHA=${{ github.sha }} ... -e GEMINI_API_KEY=${{ secrets.GEMINI_API_KEY }} ...`. Any expression inside `${{ }}` is substituted before the shell sees the command, enabling injection.

Locations:

- `.github/workflows/auto-update-linters.yml:72`

### script-injection (severity: high)

Rule (a): `${{ matrix.platform }}` is directly interpolated inside a multi-line `run:` block in the 'Prepare' step: `platform=${{ matrix.platform }}`. The `matrix.*` context is workflow-controllable and must not appear inside a run script. This also writes the derived value `PLATFORM_PAIR` to `$GITHUB_ENV` without sanitization (github-env-injection).

Locations:

- `.github/workflows/deploy-ALPHA.yml:160`
- `.github/workflows/deploy-BETA.yml:310`
- `.github/workflows/deploy-RELEASE.yml:218`

### script-injection (severity: high)

Rule (a): `${{ matrix.runner }}` and `${{ contains(fromJson(needs.get-linters-matrix.outputs.linters_arm), matrix.linter) }}` are directly interpolated inside a multi-line `run:` block in the 'Prepare' step: `if [[ "${{ matrix.runner }}" =~ $ARM_RUNNER_REGEX ]]; then` and `job_enabled="${{ contains(...) }}"`.

Locations:

- `.github/workflows/deploy-RELEASE-linters.yml:62`

### script-injection (severity: high)

Rule (a): The composite action `flavors/custom-builder/action.yml` directly interpolates multiple `${{ inputs.* }}`, `${{ github.* }}`, and `${{ env.* }}` expressions inside `run:` shell command strings. Examples include: `-v ${{ github.workspace }}:/github/workspace`, `-e GITHUB_TOKEN=${{ env.GITHUB_TOKEN }}`, `-e CUSTOM_FLAVOR_PLATFORM=${{ inputs.platform }}`, `ghcr.io/oxsecurity/megalinter-custom-flavor-builder:${{ inputs.megalinter-custom-flavor-builder-tag }}`, `REPO_OWNER=$(echo "${{ github.repository_owner }}" | tr ...)`, `if [ "${{ inputs.upload-to-ghcr }}" = "true" ]`, etc. All of these allow a calling workflow to inject arbitrary shell commands.

Locations:

- `flavors/custom-builder/action.yml:32`
- `flavors/custom-builder/action.yml:50`

### github-env-injection (severity: high)

The 'Get PR title or commit message' step writes `PR_TITLE` (sourced from `${{ github.event.pull_request.title }}`) and `HEAD_COMMIT_MSG` (sourced from `${{ github.event.head_commit.message }}`) to `$GITHUB_OUTPUT` without sanitization: `echo "title=${PR_TITLE}" >> "${GITHUB_OUTPUT}"` and `echo "title=${HEAD_COMMIT_MSG}" >> "${GITHUB_OUTPUT}"`. A PR title or commit message containing newlines could inject arbitrary output variables.

Locations:

- `.github/workflows/deploy-DEV-linters.yml:44`

### github-env-injection (severity: high)

The 'Prepare' step in the build-custom-flavor-builder job writes `PLATFORM_PAIR` (derived from `${{ matrix.platform }}` via `platform=${{ matrix.platform }}`) to `$GITHUB_ENV` without sanitization: `echo "PLATFORM_PAIR=${platform//\//-}" >> "${GITHUB_ENV}"`. A matrix value containing newlines could inject arbitrary environment variables.

Locations:

- `.github/workflows/deploy-ALPHA.yml:161`
- `.github/workflows/deploy-BETA.yml:311`
- `.github/workflows/deploy-RELEASE.yml:219`

### github-env-injection (severity: high)

The 'Prepare' step writes `job_enabled` (derived from `${{ contains(fromJson(...), matrix.linter) }}`) and `platforms` to `$GITHUB_OUTPUT` without sanitization: `echo "job_enabled=$job_enabled" >> "${GITHUB_OUTPUT}"` and `echo "platforms=$platforms" >> "${GITHUB_OUTPUT}"`. Values derived from `matrix.*` or `needs.*.outputs.*` containing newlines could inject arbitrary output variables.

Locations:

- `.github/workflows/deploy-RELEASE-linters.yml:68`

### unpinned-uses (severity: high)

The root `action.yml` and all flavor `action.yml` files use a mutable Docker image tag (`v9.6.0`) instead of a SHA digest in their `runs.image:` field. Example: `image: "docker://ghcr.io/oxsecurity/megalinter:v9.6.0"`. A tag can be silently updated to point to a different (potentially malicious) image. All affected files: action.yml, flavors/c_cpp/action.yml, flavors/ci_light/action.yml, flavors/cupcake/action.yml, flavors/documentation/action.yml, flavors/dotnet/action.yml, flavors/dotnetweb/action.yml, flavors/formatters/action.yml, flavors/go/action.yml, flavors/java/action.yml, flavors/javascript/action.yml, flavors/php/action.yml, flavors/python/action.yml, flavors/ruby/action.yml, flavors/rust/action.yml, flavors/salesforce/action.yml, flavors/security/action.yml, flavors/swift/action.yml, flavors/terraform/action.yml.

Locations:

- `action.yml:11`
- `flavors/c_cpp/action.yml:11`
- `flavors/ci_light/action.yml:11`
- `flavors/cupcake/action.yml:11`
- `flavors/documentation/action.yml:11`
- `flavors/dotnet/action.yml:11`
- `flavors/dotnetweb/action.yml:11`
- `flavors/formatters/action.yml:11`
- `flavors/go/action.yml:11`
- `flavors/java/action.yml:11`
- `flavors/javascript/action.yml:11`
- `flavors/php/action.yml:11`
- `flavors/python/action.yml:11`
- `flavors/ruby/action.yml:11`
- `flavors/rust/action.yml:11`
- `flavors/salesforce/action.yml:11`
- `flavors/security/action.yml:11`
- `flavors/swift/action.yml:11`
- `flavors/terraform/action.yml:11`

### unpinned-uses (severity: high)

Unpinned `uses:` references found in workflow files. `mega-linter.yml` uses `oxsecurity/megalinter/flavors/python@beta` (branch/tag ref, not a 40-char SHA). `mega-linter-for-runner.yml` uses `oxsecurity/megalinter/flavors/javascript@beta` (branch/tag ref). `flavors/custom-builder/action.yml` uses `actions/upload-artifact@v7` (version tag, not a SHA).

Locations:

- `.github/workflows/mega-linter.yml:68`
- `.github/workflows/mega-linter-for-runner.yml:68`
- `flavors/custom-builder/action.yml:97`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, unpinned-uses

**Notes:**

Fixed all findings:

1. script-injection in auto-update-linters.yml: Moved github.sha and secrets.GEMINI_API_KEY to env: block.

2. script-injection + github-env-injection in deploy-ALPHA/BETA/RELEASE.yml: Moved matrix.platform to env: block as MATRIX_PLATFORM, sanitized with tr -d '\n\r' before writing to GITHUB_ENV.

3. script-injection + github-env-injection in deploy-RELEASE-linters.yml: Moved matrix.runner and contains() expression to env: block, sanitized output values before writing to GITHUB_OUTPUT.

4. script-injection in flavors/custom-builder/action.yml: Moved all ${{ inputs.* }}, ${{ github.* }}, ${{ env.* }} expressions to env: blocks. Also pinned actions/upload-artifact@v7 to SHA 043fb46d1a93c77aae656e7c1c64a875d1fc6a0a.

5. github-env-injection in deploy-DEV-linters.yml: Sanitized PR_TITLE and HEAD_COMMIT_MSG with tr -d '\n\r' before writing to GITHUB_OUTPUT.

6. unpinned-uses for 19 docker action.yml files: Pinned all megalinter images to SHA digests while preserving docker:// scheme and tag.

7. unpinned-uses in mega-linter.yml and mega-linter-for-runner.yml: Pinned oxsecurity/megalinter/flavors/python@beta and oxsecurity/megalinter/flavors/javascript@beta to SHA 331108469f4b174c6ad77f8231249b331bcdb056.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection, unsafe-shell

**Notes:**

Fixed all findings:

1. script-injection (deploy-ALPHA.yml, deploy-BETA.yml, deploy-RELEASE.yml): Moved `${{ github.repository }}` from run: shell commands into env: blocks as `GH_REPOSITORY`, updated printf and docker inspect commands to use `${GH_REPOSITORY}` env var.

2. script-injection + github-env-injection (mega-linter.yml, mega-linter-for-runner.yml): Moved all `${{ steps.ml.outputs.* }}`, `${{ env.* }}`, and `${{ github.* }}` expressions from run: blocks into env: blocks. Added `printf '%s' "$VAR" | tr -d '\n\r'` sanitization before writing values to $GITHUB_ENV.

3. unsafe-shell (deploy-DEV.yml): Replaced `bash <(curl -s https://codecov.io/env)` process substitution with: download to temp file via `curl -s https://codecov.io/env -o "$CODECOV_ENV_SCRIPT"`, execute with `bash "$CODECOV_ENV_SCRIPT"`, then clean up with `rm -f`.

