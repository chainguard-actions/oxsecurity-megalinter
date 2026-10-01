<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v10.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v10.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple action.yml files reference Docker images using mutable version tags (v10.1.0) instead of immutable SHA digests. This exposes the action to supply-chain attacks if the tag is moved. Affected image references include: action.yml: `docker://ghcr.io/oxsecurity/megalinter:v10.1.0`; flavors/c_cpp/action.yml: `docker://ghcr.io/oxsecurity/megalinter-c_cpp:v10.1.0`; flavors/ci_light/action.yml: `docker://ghcr.io/oxsecurity/megalinter-ci_light:v10.1.0`; flavors/cupcake/action.yml: `docker://ghcr.io/oxsecurity/megalinter-cupcake:v10.1.0`; and all remaining flavor action.yml files similarly using `:v10.1.0` tags.

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

### script-injection (severity: high)

flavors/custom-builder/action.yml contains two run: blocks that directly interpolate ${{ }} expressions into shell commands (sub-rule a), allowing an attacker to inject arbitrary shell commands via workflow-controlled inputs or github context values.

Step 1 ('Build Custom MegaLinter Flavor', line ~37-43) interpolates: `${{ github.workspace }}`, `${{ env.GITHUB_TOKEN }}`, `${{ inputs.platform }}`, `${{ env.CUSTOM_FLAVOR_BUILD_REPO }}`, `${{ env.CUSTOM_FLAVOR_BUILD_REPO_URL }}`, `${{ env.CUSTOM_FLAVOR_BUILD_USER }}`, and `${{ inputs.megalinter-custom-flavor-builder-tag }}` directly into a docker run command.

Step 2 ('Tag and Push Docker Image', line ~49-51) interpolates: `${{ github.repository_owner }}`, `${{ github.event.repository.name }}`, `${{ github.repository }}`, `${{ inputs.upload-to-ghcr }}`, `${{ inputs.is-latest }}`, and `${{ inputs.upload-to-dockerhub }}` directly into shell commands.

Locations:

- `flavors/custom-builder/action.yml:37`
- `flavors/custom-builder/action.yml:49`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed 19 Docker image references by pinning them to immutable SHA digests (preserving docker:// scheme and tag inline). Fixed script injection in flavors/custom-builder/action.yml by moving all ${{ }} expressions from both run: blocks into env: blocks, then referencing them as plain shell environment variables.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script injection findings in hardened/action/flavors/custom-builder/action.yml:
1. Line 51 (Build Custom MegaLinter Flavor step): Assigned the docker image reference to a BUILDER_IMAGE variable first (`BUILDER_IMAGE="ghcr.io/oxsecurity/megalinter-custom-flavor-builder:${BUILDER_TAG}"`), then used `"$BUILDER_IMAGE"` in the docker run command. This prevents command substitution injection from the $BUILDER_TAG value.
2. Lines 67, 73, 74, 84, 85 (Tag and Push Docker Image step): Double-quoted all shell variables in docker commands: `docker inspect ... "$IMAGE_NAME"`, `docker tag "$IMAGE_NAME" "ghcr.io/$REPO_OWNER/$REPO_NAME/..."`, `docker push "ghcr.io/$REPO_OWNER/$REPO_NAME/..."`, `docker tag "$IMAGE_NAME" "$DOCKERHUB_IMAGE:$IMAGE_VERSION"`, `docker push "$DOCKERHUB_IMAGE:$IMAGE_VERSION"`. This prevents word splitting and shell metacharacter injection from untrusted github context values.

