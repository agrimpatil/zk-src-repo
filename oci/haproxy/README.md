# haproxy OCI build context (Party A source)

This directory is the **Kaniko build context** for the `docker-library/haproxy`
alpine image — the "source" that `f_source` commits to. It is exactly the two
files Kaniko needs: `Dockerfile` + `docker-entrypoint.sh`.

## Provenance

- Upstream: `github.com/docker-library/haproxy`
- Pinned commit: `276caff373d711bd7246fedc4fa0ba3d4186016d`
- Rendered variant: `3.2/alpine/Dockerfile` (haproxy 3.2.26, alpine:3.24)

## Party-A pinning patches (make the build reproducible)

1. **Base image by digest** — `FROM alpine:3.24@sha256:be2656061b536c1883c4034790e21d0a09c5aaaf51968b89368a3c0371ea2c9a`
   (the OCI image-index digest, arch-independent), not the mutable `:3.24` tag.
2. **`SOURCE_DATE_EPOCH 1700000000`** — fixes haproxy's embedded build date so two
   independent builds produce the same binary (and thus the same image digest).

The haproxy source tarball is content-pinned upstream by `HAPROXY_SHA256`, which lives
in this Dockerfile — so its integrity is transitively covered by `f_source`.

## Build (Kaniko, `--reproducible`)

Both Party A (reference) and Party B (enclave) build this identically; `--reproducible`
zeroes layer metadata timestamps so the OCI manifest digests match.
