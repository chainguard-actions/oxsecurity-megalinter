<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter/v8.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter/v8.8.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All action.yml files reference Docker images using mutable version tags (e.g., `:v8.8.0`) instead of immutable SHA digests. This means the action could silently pull a different (potentially malicious) image if the tag is reassigned. All 19 action.yml files are affected: the root action.yml uses `docker://oxsecurity/megalinter:v8.8.0` and each flavor uses its own tagged image (e.g., `docker://oxsecurity/megalinter-c_cpp:v8.8.0`). Each should be pinned to a full SHA256 digest, e.g., `docker://oxsecurity/megalinter@sha256:<64-hex-char-digest> # v8.8.0`.

Locations:

- `action.yml:9`
- `flavors/c_cpp/action.yml:9`
- `flavors/ci_light/action.yml:9`
- `flavors/cupcake/action.yml:9`
- `flavors/documentation/action.yml:9`
- `flavors/dotnet/action.yml:9`
- `flavors/dotnetweb/action.yml:9`
- `flavors/formatters/action.yml:9`
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

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all 19 Docker image references to immutable SHA256 digests across action.yml and all 18 flavor action.yml files. Each image now uses the format `docker://oxsecurity/megalinter[-flavor]:v8.8.0@sha256:<digest>`, preserving the `docker://` scheme and version tag while adding the immutable digest. Images pinned: megalinter (930b3db), c_cpp (108754f), ci_light (d3a7901), cupcake (9d35de9), documentation (e02f513), dotnet (4d94ac4), dotnetweb (a728e2f), formatters (d04a052), go (4e71fad), java (5f15a5f), javascript (5420702), php (920cd93), python (80412eb), ruby (8c981b7), rust (fa69634), salesforce (4586795), security (1b5cf2e), swift (4777a11), terraform (ad91bff).

