# GPU Operator (RKE2)

Deploys the **NVIDIA GPU Operator** onto RKE2 (containerd) using Rancher Fleet.

## What this stack does

- Installs the NVIDIA GPU Operator via Helm.
- Applies **RKE2/containerd**-specific toolkit configuration defaults.
- Provides two opinionated profiles:
  - `values-dev.yaml`: assumes drivers are pre-installed (fast sandbox bring-up).
  - `values-prod.yaml`: SLES-friendly production defaults (assumes pre-installed drivers, monitoring enabled).
  - `values-prod-driver.yaml`: optional layer to enable operator-managed drivers.

## Prerequisites / assumptions

- You have GPU-capable worker nodes (NVIDIA GPUs).
- Cluster is **RKE2** (containerd).
- You understand whether you want the operator to **manage drivers**:
  - Dev profile defaults to **driver disabled** (assumes pre-installed driver).
  - Prod profile defaults to **driver disabled** (SLES-friendly); optionally layer `values-prod-driver.yaml` to enable operator-managed drivers.

## RKE2 containerd paths (defaults)

These defaults are commonly referenced for RKE2:

- `CONTAINERD_CONFIG`: `/var/lib/rancher/rke2/agent/etc/containerd/config.toml.tmpl`
- `CONTAINERD_SOCKET`: `/run/k3s/containerd/containerd.sock`
- `CONTAINERD_RUNTIME_CLASS`: `nvidia`
- `CONTAINERD_SET_AS_DEFAULT`: `"false"` by default in this catalog (safer; require explicit `runtimeClassName: nvidia` for GPU workloads)

If your RKE2 differs (custom paths, hardened config, etc.), override via values.

## How to deploy with Fleet

Point Fleet at `stacks/gpu-operator/` (this folder) in your repo.

- The Fleet bundle is defined in `fleet.yaml`.
- Choose a profile by selecting the appropriate values file in your Fleet configuration:
  - dev: `values-dev.yaml`
  - prod: `values-prod.yaml`
  - prod + driver managed: `values-prod.yaml` + `values-prod-driver.yaml` (layered in that order)

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

