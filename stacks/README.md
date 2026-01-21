# Stacks

Each folder under `stacks/` is intended to be a **Fleet bundle** (i.e. it contains a `fleet.yaml` plus any referenced `values*.yaml` and optional test manifests/docs).

General conventions:

- **Pin versions**: avoid floating `latest`.
- **Dev vs prod**: provide at least `values-dev.yaml` and `values-prod.yaml`.
- **UI-friendly knobs**: keep overrides focused and document them in the stack README.

