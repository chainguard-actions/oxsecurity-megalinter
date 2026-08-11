<!-- markdownlint-disable -->

# Hardening Report: oxsecurity--megalinter--flavors--javascript/v8.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **oxsecurity--megalinter--flavors--javascript/v8.8.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses a Docker image referenced by a mutable tag rather than an immutable SHA digest. In action.yml, `image: "docker://oxsecurity/megalinter-javascript:v8.8.0"` uses the tag `v8.8.0`, which can be silently replaced by a different (potentially malicious) image at any time. It should be pinned to a specific SHA256 digest, e.g. `image: "docker://oxsecurity/megalinter-javascript@sha256:<64-hex-char-digest> # v8.8.0"`.

Locations:

- `action.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag `docker://oxsecurity/megalinter-javascript:v8.8.0` to the immutable digest `docker://oxsecurity/megalinter-javascript:v8.8.0@sha256:5420702e6fb3618e239402bc0b7b21c3752d65d55a4ce5024b8ac4dc0e3826be`. The `docker://` scheme and `:v8.8.0` tag are preserved inline alongside the digest.

