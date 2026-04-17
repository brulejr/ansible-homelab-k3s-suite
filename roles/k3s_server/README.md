# k3s_server

Host-level K3S server bootstrap role.

## Responsibility

This role owns:

- K3S server prerequisite packages
- K3S config rendering
- K3S installation
- systemd service environment configuration
- service enablement and basic verification

This role does not own:

- host-level Gitea
- Traefik deployment
- cluster applications
- agent node joins

## Required variables

- `k3s_server_cluster_init`
- `k3s_server_disable_traefik`
- `k3s_server_write_kubeconfig_mode`

## Notes

This role is intended to run after host foundation and before cluster middleware or application roles.
