<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v6** was hardened automatically. 5 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use non-SHA action refs (e.g., @v3, @v4, @master, @beta, @v2, @v1.1.0, @v0.15.2, @v2.0.2, @v3.6.0, @v4.5.0, @v8). Examples include: actions/checkout@v3, docker/setup-qemu-action@v2, docker/build-push-action@v4, aquasecurity/trivy-action@master, oxsecurity/megalinter/flavors/python@beta, pascalgn/automerge-action@v0.15.2, Actions-R-Us/actions-tagger@v2.0.2. Additionally, the root action.yml and all 15 flavor action.yml files reference Docker images by tag (e.g., docker://oxsecurity/megalinter:v6.22.2) instead of SHA digest, making them vulnerable to supply-chain attacks.

Locations:

- `.github/workflows/auto-update-linters.yml:35`
- `.github/workflows/automerge-dependabot.yml:29`
- `.github/workflows/automerge.yml:26`
- `.github/workflows/build-command.yml:44`
- `.github/workflows/build-deploy-docs.yml:22`
- `.github/workflows/deploy-ALPHA-flavors.yml:57`
- `.github/workflows/deploy-ALPHA.yml:47`
- `.github/workflows/deploy-BETA-flavors.yml:64`
- `.github/workflows/deploy-BETA-linters.yml:112`
- `.github/workflows/deploy-BETA.yml:48`
- `.github/workflows/deploy-DEV-linters.yml:113`
- `.github/workflows/deploy-DEV.yml:57`
- `.github/workflows/deploy-RELEASE-flavors.yml:52`
- `.github/workflows/deploy-RELEASE-linters.yml:112`
- `.github/workflows/deploy-RELEASE.yml:42`
- `.github/workflows/help-command.yml:26`
- `.github/workflows/mega-linter-for-runner.yml:28`
- `.github/workflows/mega-linter.yml:28`
- `.github/workflows/slash-command-dispatch.yml:9`
- `.github/workflows/stale.yml:24`
- `.github/workflows/test-mega-linter-runner.yml:33`
- `.github/workflows/versioning.yml:30`
- `action.yml:9`
- `flavors/ci_light/action.yml:9`
- `flavors/cupcake/action.yml:9`
- `flavors/documentation/action.yml:9`
- `flavors/dotnet/action.yml:9`
- `flavors/go/action.yml:9`
- `flavors/java/action.yml:9`
- `flavors/javascript/action.yml:9`
- `flavors/php/action.yml:9`
- `flavors/python/action.yml:9`
- `flavors/ruby/action.yml:9`
- `flavors/rust/action.yml:9`
- `flavors/salesforce/action.yml:9`
- `flavors/security/action.yml:9`
- `flavors/swift/action.yml:9`
- `flavors/terraform/action.yml:9`

### missing-permissions (severity: medium)

None of the 22 workflow files define a top-level `permissions:` key, and no job within any of these files defines job-level permissions. Without explicit permissions, workflows run with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/auto-update-linters.yml:1`
- `.github/workflows/automerge-dependabot.yml:1`
- `.github/workflows/automerge.yml:1`
- `.github/workflows/build-command.yml:1`
- `.github/workflows/build-deploy-docs.yml:1`
- `.github/workflows/deploy-ALPHA-flavors.yml:1`
- `.github/workflows/deploy-ALPHA.yml:1`
- `.github/workflows/deploy-BETA-flavors.yml:1`
- `.github/workflows/deploy-BETA-linters.yml:1`
- `.github/workflows/deploy-BETA.yml:1`
- `.github/workflows/deploy-DEV-linters.yml:1`
- `.github/workflows/deploy-DEV.yml:1`
- `.github/workflows/deploy-RELEASE-flavors.yml:1`
- `.github/workflows/deploy-RELEASE-linters.yml:1`
- `.github/workflows/deploy-RELEASE.yml:1`
- `.github/workflows/help-command.yml:1`
- `.github/workflows/mega-linter-for-runner.yml:1`
- `.github/workflows/mega-linter.yml:1`
- `.github/workflows/slash-command-dispatch.yml:1`
- `.github/workflows/stale.yml:1`
- `.github/workflows/test-mega-linter-runner.yml:1`
- `.github/workflows/versioning.yml:1`

### script-injection (severity: high)

Multiple workflow run: blocks directly interpolate ${{ ... }} expressions into shell commands (sub-rule a). In deploy-DEV.yml: `TAG="test-${{ github.actor }}-${BRANCH_NAME}"` (attacker-controlled github.actor injected into shell), `${{ github.event.head_commit.message }}` used in a shell string comparison, and `${{ github.head_ref }}` in a shell assignment. In deploy-DEV-linters.yml: same `${{ github.actor }}` pattern. In deploy-BETA-linters.yml and deploy-RELEASE-linters.yml: `${{ github.head_ref }}` in run blocks. In auto-update-linters.yml: `${{ github.sha }}` directly in a docker run command. In deploy-BETA.yml: `${{ github.sha }}` and `${{ secrets.PAT }}` in a run block.

Locations:

- `.github/workflows/deploy-DEV.yml:78`
- `.github/workflows/deploy-DEV.yml:154`
- `.github/workflows/deploy-DEV.yml:156`
- `.github/workflows/deploy-DEV-linters.yml:33`
- `.github/workflows/deploy-DEV-linters.yml:155`
- `.github/workflows/deploy-BETA-linters.yml:155`
- `.github/workflows/deploy-RELEASE-linters.yml:155`
- `.github/workflows/auto-update-linters.yml:57`
- `.github/workflows/deploy-BETA.yml:89`

### github-env-injection (severity: high)

In deploy-DEV.yml and deploy-DEV-linters.yml, the attacker-controlled value `${{ github.actor }}` is interpolated directly into the shell variable TAG (`TAG="test-${{ github.actor }}-${BRANCH_NAME}"`), which is then written to $GITHUB_OUTPUT via `echo "tag=${TAG}" >> "$GITHUB_OUTPUT"` without the required sanitization step (`printf '%s' ... | tr -d '\n\r'`). An attacker who controls their GitHub username could inject newlines to poison the output file.

Locations:

- `.github/workflows/deploy-DEV.yml:80`
- `.github/workflows/deploy-DEV-linters.yml:35`

### unsafe-shell (severity: high)

Two workflow run: blocks use `bash <(curl -s https://codecov.io/env)` — a process substitution that downloads content from a remote URL (codecov.io) and executes it directly in bash. This is equivalent to `curl ... | bash` and is an unsafe-shell pattern: if the remote URL is compromised or the response is tampered with in transit, arbitrary code will execute on the runner.

Locations:

- `.github/workflows/deploy-BETA.yml:90`
- `.github/workflows/deploy-DEV.yml:157`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection, github-env-injection, unsafe-shell

**Notes:**

Fixed all 5 security findings across 22 workflow files and 16 action.yml files:

1. unpinned-uses: Pinned all action references to full commit SHAs (actions/checkout@a37ce91, docker/setup-qemu-action@2b82ce8, docker/setup-buildx-action@885d146, docker/build-push-action@0a97817, docker/login-action@465a078, aquasecurity/trivy-action@2736533, actions/upload-artifact@ff15f03, peter-evans/create-pull-request@38e0b6e, actions/setup-python@d27e3f3, actions/setup-node@64ed1c7, peter-evans/create-or-update-comment@67dcc54, stefanzweifel/git-auto-commit-action@3ea6ae1, pascalgn/automerge-action@8b306ad, Actions-R-Us/actions-tagger@f411bd9, fountainhead/action-wait-for-check@297be35, actions/github-script@d7906e4, peter-evans/slash-command-dispatch@f996d7b, actions/stale@1160a22, oxsecurity/megalinter/flavors/javascript@331108469, oxsecurity/megalinter/flavors/python@331108469). All 16 action.yml Docker images pinned with sha256 digests.

2. missing-permissions: Added top-level permissions blocks to all 22 workflow files with minimal required permissions.

3. script-injection: Moved all ${{ github.actor }}, ${{ github.head_ref }}, ${{ github.sha }}, ${{ github.event.head_commit.message }}, ${{ github.event.pull_request.head.repo.full_name }}, and ${{ secrets.PAT }} expressions from run: shell strings into step env: blocks, referencing them as plain environment variables.

4. github-env-injection: In deploy-DEV.yml and deploy-DEV-linters.yml, github.actor is now sanitized with printf '%s' | tr -d '\n\r' before being written to GITHUB_OUTPUT.

5. unsafe-shell: Both instances of bash <(curl -s https://codecov.io/env) in deploy-BETA.yml and deploy-DEV.yml replaced with safe pattern: download to temp file first, then execute the file.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 8 script-injection findings across 6 workflow files:

1. **deploy-BETA-linters.yml** (line 51): Moved `${{ env.UNIQUE_DOCKER_IMAGE_NAME }}` into an `env:` block as `UNIQUE_DOCKER_IMAGE_NAME`, referenced as `"$UNIQUE_DOCKER_IMAGE_NAME"` in the run command. Also renamed the step to remove the expression from the step name.

2. **deploy-DEV.yml** (line 196): Added `IMAGE_TAG: ${{ steps.image_tag.outputs.tag }}` to the existing `env:` block of the 'Run Test Cases' step, replaced `oxsecurity/megalinter:${{ steps.image_tag.outputs.tag }}` with `"oxsecurity/megalinter:${IMAGE_TAG}"`.

3. **deploy-DEV.yml** (line 214): Added `env: DOCKER_BUILD_OUTCOME: ${{ steps.docker_build.outcome }}` to the 'debug' step, replaced `echo ${{ steps.docker_build.outcome }}` with `echo "$DOCKER_BUILD_OUTCOME"`.

4. **deploy-DEV.yml** (line 226): Added `env: MEGALINTER_RELEASE: ${{ steps.image_tag.outputs.tag }}` and `MEGALINTER_NO_DOCKER_PULL: "true"` to the 'Run mega-linter-runner tests' step, removed the inline env var assignment from the run command.

5. **deploy-DEV.yml** (bonus fix): Also fixed the 'Run against all code base' step which had the same pattern with `${{ steps.image_tag.outputs.tag }}` in a run command.

6. **deploy-RELEASE.yml** (lines 95, 103, 107, 111, 115): Added `env:` blocks with `PTAG`, `CTAG`, `PVERSION`, `CVERSION` variables to the 'Print tags', 'Checkout previous tag', 'Mike deploy previous version', 'Checkout current tag', and 'Mike deploy current version' steps, replacing all direct `${{ steps.version.outputs.* }}` interpolations.

7. **mega-linter.yml** (line 75): Added `env: PR_NUMBER: ${{ steps.cpr.outputs.pull-request-number }}` and `PR_URL: ${{ steps.cpr.outputs.pull-request-url }}` to the 'Create PR output' step, replaced direct interpolations with `$PR_NUMBER` and `$PR_URL`.

8. **mega-linter-for-runner.yml** (line 75): Same fix as mega-linter.yml.

9. **auto-update-linters.yml** (line 90): Same fix as mega-linter.yml for the 'Create PR output' step.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed all four script-injection findings across four workflow files:

1. deploy-BETA-linters.yml: Replaced `${{ matrix.linter }}` in docker image name with `${LINTER}` (env var), quoted `${GITHUB_REPOSITORY}` and `${GITHUB_BRANCH}`, and quoted `${GITHUB_WORKSPACE}` in volume mount.

2. deploy-DEV-linters.yml: Added `IMAGE_TAG: ${{ needs.prepare.outputs.tag }}` to env block, replaced `${{ matrix.linter }}` and `${{ needs.prepare.outputs.tag }}` in docker image name with `${LINTER}` and `${IMAGE_TAG}`, quoted `${GITHUB_REPOSITORY}` and `${GITHUB_BRANCH}`.

3. deploy-RELEASE-linters.yml: Replaced `${{ matrix.linter }}` and `${{ github.event.release.tag_name }}` in docker image name with `${LINTER}` and `${RELEASE_TAG}` (both already in env block), quoted `${GITHUB_REPOSITORY}` and `${GITHUB_BRANCH}`.

4. deploy-DEV.yml: Quoted `${GITHUB_REPOSITORY}` and `${GITHUB_BRANCH}` in the docker run command, and quoted `${GITHUB_WORKSPACE}` in volume mount.

All ${{ }} expressions have been moved out of run: shell strings into env: blocks, and all shell variables derived from GitHub context are now properly double-quoted to prevent shell metacharacter injection.

