# suse-ai-catalog

An opinionated, tested catalog of AI platform stacks designed to be deployed with **Rancher Fleet** onto **RKE2**.

## Goals

- Provide **known-good stacks** with sensible defaults for RKE2.
- Make it easy to run in **dev/sandbox** (fast iteration) and then **promote to production** (GitOps).
- Allow **overlays** for environment-specific values (dev/prod), with a small set of supported knobs.

## Repo layout

- `stacks/`: Fleet-ready stacks (each stack is a Fleet bundle).
- `.github/workflows/`: CI that validates stacks render and remain consistent.

## Stacks

- `stacks/gpu-operator`: NVIDIA GPU Operator with RKE2/containerd-tuned defaults.

## Notes

This repo is intended to be used in two modes:

- **Mode 1 (try-it-now)**: a UI/extension creates a Fleet `GitRepo` pointing at this repo and stores user overrides in-cluster (e.g., `GitRepo` values + Secrets).
- **Mode 2 (promote)**: overrides are committed to a downstream environment repo and Fleet points production clusters at that downstream repo.

