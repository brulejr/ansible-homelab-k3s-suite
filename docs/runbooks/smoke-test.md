# Smoke Test Runbook

This runbook validates the baseline execution flow for the `ansible-homelab-k3s-suite` when consumed from a private inventory repository.

## Goal

Confirm that the suite can orchestrate the following layers in order:

1. Host foundation
2. Host-level Gitea
3. K3S bootstrap
4. Cluster middleware
5. Cluster applications

## Assumptions

- The private inventory repository includes this suite as a submodule at `suite/`
- The private inventory repository defines:
  - `host_foundation_roles`
  - `cluster_bootstrap_roles`
  - `cluster_core_roles`
  - `cluster_app_roles`
- The target hosts are reachable over SSH
- Required secrets are defined in inventory vault data
- DNS and routing are configured appropriately for the chosen hostnames

## Example inventory shape

```yaml
all:
  children:
    gitea_hosts:
      hosts:
        idsrv01:
          ansible_host: 192.168.1.30

    k3s_servers:
      hosts:
        minisrv01:
          ansible_host: 192.168.1.10
```
