<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v9.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v9.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: Multiple ${{ }} expressions are directly interpolated inside run: shell command strings in flavors/custom-builder/action.yml. Step 'Build Custom MegaLinter Flavor' (line 33) interpolates ${{ github.workspace }}, ${{ env.GITHUB_TOKEN }}, ${{ inputs.platform }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO_URL }}, ${{ env.CUSTOM_FLAVOR_BUILD_USER }}, and ${{ inputs.megalinter-custom-flavor-builder-tag }} directly into shell commands. Step 'Tag and Push Docker Image' (line 46) interpolates ${{ github.repository_owner }}, ${{ github.event.repository.name }}, ${{ github.repository }}, ${{ inputs.upload-to-ghcr }}, ${{ inputs.is-latest }}, ${{ inputs.upload-to-dockerhub }}, and ${{ inputs.is-latest }} directly into shell commands. These allow shell metacharacter injection from attacker-controlled inputs.

Locations:

- `flavors/custom-builder/action.yml:33`
- `flavors/custom-builder/action.yml:46`

### unpinned-uses (severity: high)

Multiple action.yml files reference Docker images using mutable version tags (e.g., v9.6.0) instead of immutable SHA digests, making them vulnerable to supply-chain attacks. Affected image references include: action.yml (docker://ghcr.io/oxsecurity/megalinter:v9.6.0), flavors/c_cpp/action.yml (megalinter-c_cpp:v9.6.0), flavors/ci_light/action.yml (megalinter-ci_light:v9.6.0), flavors/cupcake/action.yml, flavors/documentation/action.yml, flavors/dotnet/action.yml, flavors/dotnetweb/action.yml, flavors/formatters/action.yml, flavors/go/action.yml, flavors/java/action.yml, flavors/javascript/action.yml, flavors/php/action.yml, flavors/python/action.yml, flavors/ruby/action.yml, flavors/rust/action.yml, flavors/salesforce/action.yml, flavors/security/action.yml, flavors/swift/action.yml, flavors/terraform/action.yml (all using :v9.6.0 tags). Additionally, flavors/custom-builder/action.yml uses actions/upload-artifact@v7 (a mutable tag, not a 40-character SHA).

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
- `flavors/custom-builder/action.yml:87`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in flavors/custom-builder/action.yml by moving all ${{ }} expressions from run: shell commands into env: blocks for both steps ('Build Custom MegaLinter Flavor' and 'Tag and Push Docker Image'). Fixed unpinned-uses by pinning all 19 Docker container image references (action.yml + 18 flavor action.yml files) with their SHA256 digests while preserving the docker:// scheme and version tags inline. Also pinned actions/upload-artifact@v7 to its full commit SHA (043fb46d1a93c77aae656e7c1c64a875d1fc6a0a).

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in the 'Tag and Push Docker Image' step of flavors/custom-builder/action.yml. All docker commands (docker inspect, docker tag, docker push) now use double-quoted variable expansions for $IMAGE_NAME, $REPO_OWNER/$REPO_NAME paths, $DOCKERHUB_IMAGE, and $IMAGE_VERSION. The github context values were already correctly placed in the env block; the fix adds double-quotes around the derived shell variables used in docker command arguments to prevent shell metacharacter injection.

