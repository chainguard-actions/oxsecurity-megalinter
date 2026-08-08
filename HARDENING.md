<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v10.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v10.0.0** was hardened automatically. 10 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: blocks. Step 'Build Custom MegaLinter Flavor' interpolates ${{ github.workspace }}, ${{ env.GITHUB_TOKEN }}, ${{ inputs.platform }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO_URL }}, ${{ env.CUSTOM_FLAVOR_BUILD_USER }}, and ${{ inputs.megalinter-custom-flavor-builder-tag }} directly in a shell run: block.

Locations:

- `flavors/custom-builder/action.yml:32`

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: blocks. Step 'Tag and Push Docker Image' interpolates ${{ github.repository_owner }}, ${{ github.event.repository.name }}, ${{ github.repository }}, ${{ inputs.upload-to-ghcr }}, ${{ inputs.is-latest }}, and ${{ inputs.upload-to-dockerhub }} directly in a shell run: block.

Locations:

- `flavors/custom-builder/action.yml:47`

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: block. Step 'Collect latest versions and help' interpolates ${{ github.sha }} and ${{ secrets.GEMINI_API_KEY }} directly in a docker run shell command: 'run: docker run -e GITHUB_SHA=${{ github.sha }} -e GEMINI_API_KEY=${{ secrets.GEMINI_API_KEY }} ... oxsecurity/megalinter:auto_update_${{ github.sha }}'.

Locations:

- `.github/workflows/auto-update-linters.yml:76`

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: block. Step 'Prepare' interpolates ${{ matrix.platform }} directly in a shell run: block: 'platform=${{ matrix.platform }}'.

Locations:

- `.github/workflows/deploy-ALPHA.yml:131`
- `.github/workflows/deploy-BETA.yml:258`
- `.github/workflows/deploy-BETA-linters.yml:79`
- `.github/workflows/deploy-RELEASE.yml:183`

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: block. Step 'Create manifest list and push' interpolates ${{ github.repository }} directly in a shell printf command: "printf 'ghcr.io/${{ github.repository }}-custom-flavor-builder@sha256:%s ' *".

Locations:

- `.github/workflows/deploy-ALPHA.yml:207`
- `.github/workflows/deploy-BETA.yml:330`
- `.github/workflows/deploy-RELEASE.yml:258`

### script-injection (severity: high)

Sub-rule (a): Direct ${{ }} expression interpolation in run: blocks. Steps 'Set APPLY_FIXES_IF var' and 'Set APPLY_FIXES_IF_* vars' interpolate ${{ steps.ml.outputs.has_updated_sources }}, ${{ env.APPLY_FIXES_EVENT }}, ${{ github.event_name }}, ${{ github.event.pull_request.head.repo.full_name }}, ${{ github.repository }}, ${{ env.APPLY_FIXES_IF }}, ${{ env.APPLY_FIXES_MODE }}, and ${{ github.ref }} directly in printf shell commands that write to $GITHUB_ENV.

Locations:

- `.github/workflows/mega-linter.yml:163`
- `.github/workflows/mega-linter.yml:175`
- `.github/workflows/mega-linter-for-runner.yml:113`
- `.github/workflows/mega-linter-for-runner.yml:125`

### github-env-injection (severity: high)

Steps 'Set APPLY_FIXES_IF var' and 'Set APPLY_FIXES_IF_* vars' write values derived from ${{ github.event_name }}, ${{ github.event.pull_request.head.repo.full_name }}, ${{ github.repository }}, ${{ env.APPLY_FIXES_EVENT }}, ${{ env.APPLY_FIXES_IF }}, ${{ env.APPLY_FIXES_MODE }}, and ${{ github.ref }} to $GITHUB_ENV using printf without the required sanitization step (printf '%s' ... | tr -d '\n\r').

Locations:

- `.github/workflows/mega-linter.yml:163`
- `.github/workflows/mega-linter.yml:175`
- `.github/workflows/mega-linter-for-runner.yml:113`
- `.github/workflows/mega-linter-for-runner.yml:125`

### github-env-injection (severity: high)

Step 'Prepare' assigns ${{ matrix.platform }} to a shell variable and then writes a derived value (PLATFORM_PAIR) to $GITHUB_ENV via 'echo "PLATFORM_PAIR=${platform//\//-}" >> "${GITHUB_ENV}"' without sanitizing the matrix.platform value with 'printf "%s" | tr -d "\n\r"' first.

Locations:

- `.github/workflows/deploy-ALPHA.yml:131`
- `.github/workflows/deploy-BETA.yml:258`
- `.github/workflows/deploy-BETA-linters.yml:79`
- `.github/workflows/deploy-RELEASE.yml:183`

### unpinned-uses (severity: high)

The action uses a Docker image referenced by a mutable version tag (v10.0.0) instead of an immutable SHA digest. All action.yml files in the root and flavor directories use 'image: docker://ghcr.io/oxsecurity/megalinter*:v10.0.0' which is a tag reference, not a SHA digest (e.g., sha256:<64-hex-char-digest>).

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

Workflow files use unpinned (tag-based) 'uses:' references: 'uses: oxsecurity/megalinter/flavors/python@beta' and 'uses: oxsecurity/megalinter/flavors/javascript@beta'. These should be pinned to a full 40-character commit SHA.

Locations:

- `.github/workflows/mega-linter.yml:88`
- `.github/workflows/mega-linter-for-runner.yml:73`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all findings across multiple files:

1. **unpinned-uses (container images)**: Pinned all 19 flavor action.yml files and the main action.yml with sha256 digests for their Docker image references (ghcr.io/oxsecurity/megalinter*:v10.0.0@sha256:...).

2. **unpinned-uses (workflow uses)**: Pinned oxsecurity/megalinter/flavors/python@beta and oxsecurity/megalinter/flavors/javascript@beta to full commit SHA 331108469f4b174c6ad77f8231249b331bcdb056 in mega-linter.yml and mega-linter-for-runner.yml.

3. **script-injection in flavors/custom-builder/action.yml**: Moved all ${{ }} expressions (github.workspace, env.GITHUB_TOKEN, inputs.platform, env.CUSTOM_FLAVOR_BUILD_REPO, env.CUSTOM_FLAVOR_BUILD_REPO_URL, env.CUSTOM_FLAVOR_BUILD_USER, inputs.megalinter-custom-flavor-builder-tag, github.repository_owner, github.event.repository.name, github.repository, inputs.upload-to-ghcr, inputs.is-latest, inputs.upload-to-dockerhub) to env: blocks.

4. **script-injection in auto-update-linters.yml**: Moved ${{ github.sha }} and ${{ secrets.GEMINI_API_KEY }} to env: block in the 'Collect latest versions and help' step.

5. **script-injection + github-env-injection in deploy-ALPHA.yml, deploy-BETA.yml, deploy-BETA-linters.yml, deploy-RELEASE.yml**: Fixed 'Prepare' step to use env: block for matrix.platform and sanitize with tr -d '\n\r' before writing to GITHUB_ENV.

6. **script-injection in deploy-ALPHA.yml, deploy-BETA.yml, deploy-RELEASE.yml**: Fixed 'Create manifest list and push' step to use env: block for github.repository.

7. **script-injection + github-env-injection in mega-linter.yml and mega-linter-for-runner.yml**: Refactored 'Set APPLY_FIXES_IF var' and 'Set APPLY_FIXES_IF_* vars' steps to move all ${{ }} expressions to env: blocks and sanitize values with printf '%s' | tr -d '\n\r' before writing to GITHUB_ENV.

