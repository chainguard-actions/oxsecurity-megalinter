<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v10.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v10.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: Direct ${{ }} expression interpolation inside run: shell commands in flavors/custom-builder/action.yml. Step 'Build Custom MegaLinter Flavor' interpolates ${{ github.workspace }}, ${{ env.GITHUB_TOKEN }}, ${{ inputs.platform }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO }}, ${{ env.CUSTOM_FLAVOR_BUILD_REPO_URL }}, ${{ env.CUSTOM_FLAVOR_BUILD_USER }}, and ${{ inputs.megalinter-custom-flavor-builder-tag }} directly into shell commands. Step 'Tag and Push Docker Image' interpolates ${{ github.repository_owner }}, ${{ github.event.repository.name }}, ${{ github.repository }}, ${{ inputs.upload-to-ghcr }}, ${{ inputs.is-latest }}, and ${{ inputs.upload-to-dockerhub }} directly into shell commands. These values flow through YAML template substitution before the shell sees them, enabling command injection.

Locations:

- `flavors/custom-builder/action.yml:37`
- `flavors/custom-builder/action.yml:38`
- `flavors/custom-builder/action.yml:39`
- `flavors/custom-builder/action.yml:40`
- `flavors/custom-builder/action.yml:41`
- `flavors/custom-builder/action.yml:42`
- `flavors/custom-builder/action.yml:43`
- `flavors/custom-builder/action.yml:48`
- `flavors/custom-builder/action.yml:49`
- `flavors/custom-builder/action.yml:50`
- `flavors/custom-builder/action.yml:55`
- `flavors/custom-builder/action.yml:59`
- `flavors/custom-builder/action.yml:67`
- `flavors/custom-builder/action.yml:72`

### unpinned-uses (severity: high)

All docker-based action.yml files reference the MegaLinter Docker image using a mutable version tag (e.g., 'docker://ghcr.io/oxsecurity/megalinter:v10.0.0') instead of an immutable SHA digest. This allows the image to be replaced with a different version without changing the action reference, creating a supply-chain risk. All flavor action.yml files are affected with the same pattern.

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

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in flavors/custom-builder/action.yml by moving all ${{ }} expressions from both run: steps into env: blocks. Fixed unpinned-uses by pinning all 19 docker container image references (action.yml + 18 flavor action.yml files) to their immutable SHA256 digests, preserving the docker:// scheme and v10.0.0 tag inline.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in flavors/custom-builder/action.yml. All docker commands now properly double-quote variables derived from github context and inputs: `$IMAGE_NAME` in `docker inspect`, `docker tag`, and `docker push` commands; composite image reference strings for GHCR (`ghcr.io/$REPO_OWNER/$REPO_NAME/megalinter-custom-flavor:$IMAGE_VERSION`) and DockerHub (`$DOCKERHUB_IMAGE:$IMAGE_VERSION`) are now wrapped in double quotes. The `$INPUT_BUILDER_TAG` on line 49 was already inside a double-quoted string and was already protected from word splitting/globbing.

