# Docker Konflux Dependencies

## Overview

The Konflux hermetic builds use two Dockerfiles:

- `Dockerfile.epp.konflux` — builds the EPP (Endpoint Picker) binary
- `Dockerfile.sidecar.konflux` — builds the PD-Sidecar (Routing Sidecar) binary

Both are pure Go builds using `registry.access.redhat.com/ubi9/go-toolset` as the builder image. Dependencies are managed via Go modules (`go.mod` / `go.sum`) and prefetched by cachi2 during the Konflux build.

## Prefetch Configuration

The Tekton pipeline configs in `.tekton/` define a `prefetch-input` with the following dependency types:

- **gomod** — Go module dependencies (from `go.mod` / `go.sum`)
- **generic** — generic dependencies

These are prefetched and cached before the Docker build runs in a network-isolated (hermetic) environment.

## Base Images

| Image | Usage |
|-------|-------|
| `ubi9/go-toolset` | Builder stage (both EPP and sidecar) |
| `ubi9/ubi-minimal` | Runtime stage (EPP) |
| `ubi9/ubi-micro` | Runtime stage (sidecar) |

All base images are pinned to specific SHA digests for reproducible builds.

## Related Files

- `Dockerfile.epp.konflux` — EPP build file
- `Dockerfile.sidecar.konflux` — Sidecar build file
- `.tekton/odh-llm-d-inference-scheduler-pull-request.yaml` — EPP Tekton pipeline config
- `.tekton/odh-llm-d-routing-sidecar-pull-request.yaml` — Sidecar Tekton pipeline config
- `go.mod` / `go.sum` — Go module dependencies
