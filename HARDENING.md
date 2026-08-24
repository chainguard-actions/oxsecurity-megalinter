<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v8.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v8.8.0** was hardened automatically. 16 finding(s) were identified and resolved across 5 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses a mutable Docker image tag instead of a SHA digest: `image: "docker://oxsecurity/megalinter:v8.8.0"`. This is vulnerable to supply-chain attacks as the tag can be moved to a different image.

Locations:

- `action.yml:11`

### unpinned-uses (severity: high)

Multiple workflow files use unpinned (non-SHA) `uses:` references including tags, version strings, and branch names. Examples: actions/checkout@v4, docker/setup-qemu-action@v3, docker/setup-buildx-action@v3, docker/build-push-action@v6, docker/login-action@v3, docker/metadata-action@v5.7.0, actions/upload-artifact@v4, actions/setup-node@v4.4.0, actions/setup-python@v5, peter-evans/create-pull-request@v7, peter-evans/create-or-update-comment@v4, stefanzweifel/git-auto-commit-action@v6, benc-uk/workflow-dispatch@v1, aquasecurity/trivy-action@master (branch!), oxsecurity/megalinter/flavors/python@beta (branch!), oxsecurity/megalinter/flavors/javascript@beta (branch!), Actions-R-Us/actions-tagger@v2.0.3, actions/github-script@v7, peter-evans/slash-command-dispatch@v4, actions/stale@v9, nick-invision/retry@v3.

Locations:

- `.github/workflows/auto-update-linters.yml:44`
- `.github/workflows/auto-update-linters.yml:52`
- `.github/workflows/auto-update-linters.yml:55`
- `.github/workflows/auto-update-linters.yml:60`
- `.github/workflows/auto-update-linters.yml:76`
- `.github/workflows/auto-update-linters.yml:91`
- `.github/workflows/build-command.yml:38`
- `.github/workflows/build-command.yml:55`
- `.github/workflows/build-command.yml:62`
- `.github/workflows/build-command.yml:72`
- `.github/workflows/build-command.yml:82`
- `.github/workflows/build-command.yml:91`
- `.github/workflows/build-command.yml:100`
- `.github/workflows/build-command.yml:110`
- `.github/workflows/build-command.yml:120`
- `.github/workflows/build-deploy-docs.yml:20`
- `.github/workflows/build-deploy-docs.yml:23`
- `.github/workflows/deploy-ALPHA.yml:52`
- `.github/workflows/deploy-ALPHA.yml:56`
- `.github/workflows/deploy-ALPHA.yml:60`
- `.github/workflows/deploy-ALPHA.yml:64`
- `.github/workflows/deploy-ALPHA.yml:71`
- `.github/workflows/deploy-ALPHA.yml:84`
- `.github/workflows/deploy-ALPHA-flavors.yml:68`
- `.github/workflows/deploy-ALPHA-flavors.yml:71`
- `.github/workflows/deploy-ALPHA-flavors.yml:74`
- `.github/workflows/deploy-ALPHA-flavors.yml:77`
- `.github/workflows/deploy-ALPHA-flavors.yml:90`
- `.github/workflows/deploy-ALPHA-flavors.yml:104`
- `.github/workflows/deploy-ALPHA-flavors.yml:117`
- `.github/workflows/deploy-BETA.yml:55`
- `.github/workflows/deploy-BETA.yml:59`
- `.github/workflows/deploy-BETA.yml:63`
- `.github/workflows/deploy-BETA.yml:67`
- `.github/workflows/deploy-BETA.yml:71`
- `.github/workflows/deploy-BETA.yml:75`
- `.github/workflows/deploy-BETA.yml:79`
- `.github/workflows/deploy-BETA.yml:83`
- `.github/workflows/deploy-BETA.yml:87`
- `.github/workflows/deploy-BETA.yml:91`
- `.github/workflows/deploy-BETA.yml:95`
- `.github/workflows/deploy-BETA.yml:99`
- `.github/workflows/deploy-BETA.yml:130`
- `.github/workflows/deploy-BETA.yml:145`
- `.github/workflows/deploy-BETA-flavors.yml:89`
- `.github/workflows/deploy-BETA-flavors.yml:93`
- `.github/workflows/deploy-BETA-flavors.yml:97`
- `.github/workflows/deploy-BETA-flavors.yml:101`
- `.github/workflows/deploy-BETA-flavors.yml:105`
- `.github/workflows/deploy-BETA-flavors.yml:109`
- `.github/workflows/deploy-BETA-flavors.yml:113`
- `.github/workflows/deploy-BETA-flavors.yml:117`
- `.github/workflows/deploy-BETA-flavors.yml:121`
- `.github/workflows/deploy-BETA-flavors.yml:130`
- `.github/workflows/deploy-BETA-flavors.yml:134`
- `.github/workflows/deploy-BETA-flavors.yml:155`
- `.github/workflows/deploy-BETA-flavors.yml:170`
- `.github/workflows/deploy-BETA-linters.yml:175`
- `.github/workflows/deploy-BETA-linters.yml:179`
- `.github/workflows/deploy-BETA-linters.yml:183`
- `.github/workflows/deploy-BETA-linters.yml:187`
- `.github/workflows/deploy-BETA-linters.yml:191`
- `.github/workflows/deploy-BETA-linters.yml:195`
- `.github/workflows/deploy-BETA-linters.yml:199`
- `.github/workflows/deploy-BETA-linters.yml:218`
- `.github/workflows/deploy-BETA-linters.yml:232`
- `.github/workflows/deploy-DEV.yml:56`
- `.github/workflows/deploy-DEV.yml:60`
- `.github/workflows/deploy-DEV.yml:64`
- `.github/workflows/deploy-DEV.yml:68`
- `.github/workflows/deploy-DEV.yml:72`
- `.github/workflows/deploy-DEV.yml:76`
- `.github/workflows/deploy-DEV.yml:130`
- `.github/workflows/deploy-DEV.yml:155`
- `.github/workflows/deploy-DEV-linters.yml:155`
- `.github/workflows/deploy-DEV-linters.yml:159`
- `.github/workflows/deploy-DEV-linters.yml:163`
- `.github/workflows/deploy-DEV-linters.yml:167`
- `.github/workflows/deploy-DEV-linters.yml:171`
- `.github/workflows/deploy-DEV-linters.yml:185`
- `.github/workflows/deploy-RELEASE.yml:47`
- `.github/workflows/deploy-RELEASE.yml:51`
- `.github/workflows/deploy-RELEASE.yml:55`
- `.github/workflows/deploy-RELEASE.yml:59`
- `.github/workflows/deploy-RELEASE.yml:63`
- `.github/workflows/deploy-RELEASE.yml:67`
- `.github/workflows/deploy-RELEASE.yml:71`
- `.github/workflows/deploy-RELEASE.yml:75`
- `.github/workflows/deploy-RELEASE.yml:79`
- `.github/workflows/deploy-RELEASE.yml:83`
- `.github/workflows/deploy-RELEASE.yml:87`
- `.github/workflows/deploy-RELEASE.yml:91`
- `.github/workflows/deploy-RELEASE.yml:130`
- `.github/workflows/deploy-RELEASE.yml:145`
- `.github/workflows/deploy-RELEASE-flavors.yml:68`
- `.github/workflows/deploy-RELEASE-flavors.yml:72`
- `.github/workflows/deploy-RELEASE-flavors.yml:76`
- `.github/workflows/deploy-RELEASE-flavors.yml:80`
- `.github/workflows/deploy-RELEASE-flavors.yml:84`
- `.github/workflows/deploy-RELEASE-flavors.yml:88`
- `.github/workflows/deploy-RELEASE-flavors.yml:92`
- `.github/workflows/deploy-RELEASE-flavors.yml:96`
- `.github/workflows/deploy-RELEASE-flavors.yml:100`
- `.github/workflows/deploy-RELEASE-flavors.yml:115`
- `.github/workflows/deploy-RELEASE-linters.yml:155`
- `.github/workflows/deploy-RELEASE-linters.yml:159`
- `.github/workflows/deploy-RELEASE-linters.yml:163`
- `.github/workflows/deploy-RELEASE-linters.yml:167`
- `.github/workflows/deploy-RELEASE-linters.yml:171`
- `.github/workflows/deploy-RELEASE-linters.yml:185`
- `.github/workflows/gitpod.yml:22`
- `.github/workflows/gitpod.yml:26`
- `.github/workflows/gitpod.yml:38`
- `.github/workflows/help-command.yml:35`
- `.github/workflows/mega-linter.yml:55`
- `.github/workflows/mega-linter.yml:59`
- `.github/workflows/mega-linter.yml:63`
- `.github/workflows/mega-linter.yml:67`
- `.github/workflows/mega-linter.yml:71`
- `.github/workflows/mega-linter-for-runner.yml:55`
- `.github/workflows/mega-linter-for-runner.yml:59`
- `.github/workflows/mega-linter-for-runner.yml:63`
- `.github/workflows/mega-linter-for-runner.yml:67`
- `.github/workflows/mega-linter-for-runner.yml:71`
- `.github/workflows/mirror-docker-image.yml:28`
- `.github/workflows/mirror-docker-image.yml:28`
- `.github/workflows/mirror-docker-image.yml:28`
- `.github/workflows/slash-command-dispatch.yml:20`
- `.github/workflows/slash-command-dispatch.yml:43`
- `.github/workflows/slash-command-dispatch.yml:55`
- `.github/workflows/slash-command-dispatch.yml:62`
- `.github/workflows/stale.yml:20`
- `.github/workflows/test-mega-linter-runner.yml:28`
- `.github/workflows/test-mega-linter-runner.yml:31`
- `.github/workflows/test-mkdocs.yml:17`
- `.github/workflows/test-mkdocs.yml:18`
- `.github/workflows/versioning.yml:22`
- `.github/workflows/versioning.yml:26`

### broad-permissions (severity: medium)

deploy-DEV.yml sets `permissions: read-all` at the job level, which grants overly broad read access to all scopes. Should be replaced with specific minimal permissions.

Locations:

- `.github/workflows/deploy-DEV.yml:40`

### broad-permissions (severity: medium)

deploy-DEV-linters.yml sets `permissions: read-all` at the job level, which grants overly broad read access to all scopes. Should be replaced with specific minimal permissions.

Locations:

- `.github/workflows/deploy-DEV-linters.yml:27`

### broad-permissions (severity: medium)

test-mkdocs.yml sets `permissions: read-all` at the job level, which grants overly broad read access to all scopes. Should be replaced with specific minimal permissions.

Locations:

- `.github/workflows/test-mkdocs.yml:17`

### broad-permissions (severity: medium)

versioning.yml sets `permissions: write-all` at the job level, which grants overly broad write access to all scopes. Should be replaced with specific minimal permissions.

Locations:

- `.github/workflows/versioning.yml:21`

### missing-permissions (severity: medium)

gitpod.yml has no top-level `permissions:` key and the single job `build` has no job-level `permissions:` key. This defaults to the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/gitpod.yml:1`

### missing-permissions (severity: medium)

mirror-docker-image.yml has no top-level `permissions:` key and none of its three jobs (copy-to-docker-hub-alpha, copy-to-docker-hub-main, copy-to-docker-hub-release) have job-level `permissions:` keys.

Locations:

- `.github/workflows/mirror-docker-image.yml:1`

### missing-permissions (severity: medium)

test-mega-linter-runner.yml has no top-level `permissions:` key and the single job `build` has no job-level `permissions:` key.

Locations:

- `.github/workflows/test-mega-linter-runner.yml:1`

### script-injection (severity: high)

Rule (a): mirror-docker-image.yml directly interpolates `${{ github.event.inputs.source-image }}` and `${{ github.event.inputs.target-image }}` (workflow_dispatch inputs, attacker-controllable) inside `run:` shell commands: `docker pull "${{ github.event.inputs.source-image }}"`, `docker tag "${{ github.event.inputs.source-image }}" "${{ github.event.inputs.target-image }}"`, `docker push "${{ github.event.inputs.target-image }}"`. This occurs in all three jobs.

Locations:

- `.github/workflows/mirror-docker-image.yml:33`
- `.github/workflows/mirror-docker-image.yml:35`
- `.github/workflows/mirror-docker-image.yml:37`
- `.github/workflows/mirror-docker-image.yml:60`
- `.github/workflows/mirror-docker-image.yml:62`
- `.github/workflows/mirror-docker-image.yml:64`
- `.github/workflows/mirror-docker-image.yml:87`
- `.github/workflows/mirror-docker-image.yml:89`
- `.github/workflows/mirror-docker-image.yml:91`

### script-injection (severity: high)

Rule (a): deploy-BETA-linters.yml directly interpolates `${{ github.event_name }}`, `${{ github.event.pull_request.head.repo.full_name }}`, `${{ github.head_ref }}`, `${{ github.ref_name }}`, and `${{ matrix.linter }}` inside a `run:` shell block. For example: `GITHUB_BRANCH=$([ "${{ github.event_name }}" == "pull_request" ] && echo "${{ github.head_ref }}" || echo "${{ github.ref_name }}")` and `TEST_KEYWORDS_TO_USE_UPPER="${{ matrix.linter }}"`.

Locations:

- `.github/workflows/deploy-BETA-linters.yml:207`

### script-injection (severity: high)

Rule (a): deploy-DEV-linters.yml directly interpolates `${{ github.event_name }}`, `${{ github.event.pull_request.head.repo.full_name }}`, `${{ github.head_ref }}`, `${{ github.ref_name }}`, and `${{ matrix.linter }}` inside a `run:` shell block.

Locations:

- `.github/workflows/deploy-DEV-linters.yml:177`

### script-injection (severity: high)

Rule (a): deploy-DEV.yml directly interpolates `${{ github.event_name }}`, `${{ github.event.pull_request.head.repo.full_name }}`, `${{ github.head_ref }}`, `${{ github.ref_name }}`, `${{ github.event.head_commit.message }}`, `${{ steps.docker_build.outcome }}`, and `${{ fromJson(steps.meta.outputs.json).tags[0] }}` inside `run:` shell blocks. Notably `${{ github.event.head_commit.message }}` is used in a conditional string comparison and assigned to a shell variable without quoting the expression.

Locations:

- `.github/workflows/deploy-DEV.yml:155`
- `.github/workflows/deploy-DEV.yml:185`
- `.github/workflows/deploy-DEV.yml:200`
- `.github/workflows/deploy-DEV.yml:220`

### script-injection (severity: high)

Rule (a): deploy-RELEASE-linters.yml directly interpolates `${{ github.event_name }}`, `${{ github.event.pull_request.head.repo.full_name }}`, `${{ github.head_ref }}`, `${{ github.ref_name }}`, and `${{ matrix.linter }}` inside a `run:` shell block.

Locations:

- `.github/workflows/deploy-RELEASE-linters.yml:195`

### script-injection (severity: high)

Rule (a): mega-linter.yml and mega-linter-for-runner.yml directly interpolate `${{ steps.cpr.outputs.pull-request-number }}` and `${{ steps.cpr.outputs.pull-request-url }}` (steps.*.outputs.* context) inside `run:` echo commands.

Locations:

- `.github/workflows/mega-linter.yml:148`
- `.github/workflows/mega-linter-for-runner.yml:148`

### unsafe-shell (severity: high)

gitpod.yml pipes remote content directly to a shell interpreter: `curl -LsSf https://astral.sh/uv/install.sh | sh -s -- --help` and `curl -LsSf "https://astral.sh/uv/${UV_VERSION}/install.sh" | sh -s -- -v`. The script is not downloaded to a file first before execution.

Locations:

- `.github/workflows/gitpod.yml:36`
- `.github/workflows/gitpod.yml:37`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, broad-permissions, missing-permissions, script-injection, unsafe-shell

**Notes:**

Fixed all security findings across 23 workflow files and action.yml:

1. **action.yml**: Pinned oxsecurity/megalinter:v8.8.0 Docker image to SHA digest.

2. **unpinned-uses**: Pinned all 21 unique action references across all workflow files to their full commit SHAs, including: actions/checkout, docker/* actions, actions/upload-artifact, actions/setup-node, actions/setup-python, peter-evans/* actions, stefanzweifel/git-auto-commit-action, benc-uk/workflow-dispatch, aquasecurity/trivy-action, oxsecurity/megalinter flavors (beta branch), Actions-R-Us/actions-tagger, actions/github-script, peter-evans/slash-command-dispatch, actions/stale, nick-invision/retry.

3. **broad-permissions**: Replaced `permissions: read-all` in deploy-DEV.yml, deploy-DEV-linters.yml, test-mkdocs.yml with `contents: read` (and `packages: read` where needed). Replaced `permissions: write-all` in versioning.yml with `contents: write, actions: write`.

4. **missing-permissions**: Added `permissions: contents: read` to gitpod.yml and test-mega-linter-runner.yml. Added `permissions: {}` at top level and `contents: read` per-job to mirror-docker-image.yml.

5. **script-injection**: Moved all `${{ github.* }}`, `${{ steps.*.outputs.* }}`, `${{ matrix.* }}`, and `${{ fromJson(...) }}` expressions out of `run:` shell strings into `env:` blocks in: mirror-docker-image.yml, deploy-BETA-linters.yml, deploy-DEV-linters.yml, deploy-DEV.yml, deploy-RELEASE-linters.yml, mega-linter.yml, mega-linter-for-runner.yml.

6. **unsafe-shell**: Fixed gitpod.yml by downloading install scripts to temp files with `curl -o` before executing with `sh`, removing the `curl | sh` pipe pattern. Dropped the `--` separator (which was the shell's option terminator, not the script's) per the hardening rules.

### Iteration 2

**Fixes applied:** unpinned-uses, script-injection, github-env-injection

**Notes:**

Fixed all three findings:

1. unpinned-uses: Pinned all 18 flavor action.yml docker images with SHA digests (c_cpp, ci_light, cupcake, documentation, dotnet, dotnetweb, formatters, go, java, javascript, php, python, ruby, rust, salesforce, security, swift, terraform).

2. script-injection: Fixed in 6 locations across 4 workflow files:
   - auto-update-linters.yml: Moved github.sha to GH_SHA env var in docker run command; moved step outputs to PR_NUMBER/PR_URL env vars in echo step
   - deploy-DEV.yml: Moved secrets.GITHUB_TOKEN to GH_TOKEN env var in both 'Run Test Cases' and 'Run against all code base' steps
   - deploy-RELEASE.yml: Moved step outputs (ptag/ctag/pversion/cversion) to env vars in Print tags, git checkout, and mike deploy steps
   - mega-linter.yml and mega-linter-for-runner.yml: Moved all ${{ }} expressions in printf/GITHUB_ENV steps to env vars

3. github-env-injection: Fixed in mega-linter.yml and mega-linter-for-runner.yml by moving expressions to env vars (APPLY_FIXES_IF_VAL, APPLY_FIXES_IF_PR_VAL, APPLY_FIXES_IF_COMMIT_VAL) and sanitizing with `printf '%s' "$VAR" | tr -d '\n\r'` before writing to GITHUB_ENV.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed script-injection in three workflow files by moving `${{ secrets.GITHUB_TOKEN }}` out of `run:` shell strings and into the step's `env:` block as `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`. Updated the `docker run` command in each file to reference `-e GITHUB_TOKEN="$GH_TOKEN"` instead of the inline expression. Files fixed: .github/workflows/deploy-BETA-linters.yml (line 196), .github/workflows/deploy-DEV-linters.yml (line 172), .github/workflows/deploy-RELEASE-linters.yml (line 196).

### Iteration 4

**Fixes applied:** unsafe-shell

**Notes:**

Fixed the unsafe-shell finding in .github/workflows/deploy-DEV.yml at line 148. Replaced `bash <(curl -s https://codecov.io/env)` (process substitution that downloads and executes remote content) with a safe pattern: download the script to a temp file via `curl -s https://codecov.io/env -o "$CODECOV_ENV_SCRIPT"`, execute it with `bash "$CODECOV_ENV_SCRIPT"`, then clean up with `rm -f "$CODECOV_ENV_SCRIPT"`. The original behavior is preserved while eliminating the unsafe remote code execution pattern.

### Iteration 5

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions of attacker-controlled data in 'Run Test Cases' steps across all four workflow files. Changed `-e GITHUB_REPOSITORY=${GITHUB_REPOSITORY} -e GITHUB_BRANCH=${GITHUB_BRANCH}` to `-e GITHUB_REPOSITORY="${GITHUB_REPOSITORY}" -e GITHUB_BRANCH="${GITHUB_BRANCH}"` in: deploy-DEV.yml, deploy-DEV-linters.yml, deploy-BETA-linters.yml, and deploy-RELEASE-linters.yml. These variables are derived from attacker-controlled PR context values (github.event.pull_request.head.repo.full_name and github.head_ref), so double-quoting prevents shell metacharacter injection.

