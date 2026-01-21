# GPU Operator (RKE2)

Deploys the **NVIDIA GPU Operator** onto RKE2 (containerd) using Rancher Fleet.

## What this stack does

- Applies an **RKE2-native** `HelmChart` custom resource (`helm.cattle.io/v1`) that installs the GPU Operator.
- Uses the same installation mechanism as the working manual manifest you validated on SLES/RKE2.
- Sets minimal required values:
  - `driver.enabled: false`
  - `toolkit.env.CONTAINERD_SOCKET=/run/k3s/containerd/containerd.sock`

## Prerequisites / assumptions

- You have GPU-capable worker nodes (NVIDIA GPUs).
- Cluster is **RKE2** (containerd).

## RKE2 containerd paths (defaults)

This stack only sets:

- `CONTAINERD_SOCKET`: `/run/k3s/containerd/containerd.sock`

## How to deploy with Fleet

Point Fleet at `stacks/gpu-operator/` (this folder) in your repo.

- The Fleet bundle is defined in `fleet.yaml`.
- The RKE2 `HelmChart` resource is defined in `helmchart.yaml`.
- To customize values, edit `spec.valuesContent` in `helmchart.yaml` (or overlay it in a downstream repo).

## Smoke test (optional)

`tests/cuda-smoke.yaml` provides a minimal GPU pod you can apply after install to validate:

- GPU resource shows up (`nvidia.com/gpu`)
- runtime class works (`runtimeClassName: nvidia`)
- container can run `nvidia-smi`

## References

- RKE2 GPU operator guidance: `https://docs.rke2.io/add-ons/gpu_operators`
- NVIDIA GPU Operator docs: `https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/`

## Versioning note

The GPU Operator Helm chart versions in the NGC repo are typically formatted like `v25.10.1` (not `0.x`).

