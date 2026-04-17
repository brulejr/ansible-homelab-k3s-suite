# ansible-homelab-k3s-suite

Reusable Ansible suite for a K3S-based homelab.

This public repository provides:
- host-level roles for foundational services such as Gitea
- K3S bootstrap roles
- Kubernetes-owned middleware roles such as Traefik
- self-contained application roles for Kubernetes workloads

## Current role set

### Host roles
- `gitea_host`

### Cluster bootstrap roles
- `k3s_server`

### Middleware / platform roles
- `traefik`

### Sample application roles
- `plantuml`
- `mqtt`

## Design rules

- Application roles are self-contained.
- Shared middleware and cross-cutting services are separate platform roles.
- Inventory, secrets, and installation ordering belong in the private inventory repository.
- This repository should remain reusable and free of environment-specific secrets.

## Suggested orchestration order

1. Host foundation
2. Host-level Gitea
3. K3S bootstrap
4. Cluster middleware
5. Cluster applications

## Notes

These roles are intentionally shells. They provide structure, variable contracts, and task flow, but not a complete production implementation yet.
