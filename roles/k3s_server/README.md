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

## Key variables

### Core

- `k3s_server_enabled`
- `k3s_server_version`
- `k3s_server_channel`
- `k3s_server_cluster_init`
- `k3s_server_token`

### Service/config

- `k3s_server_service_name`
- `k3s_server_config_dir`
- `k3s_server_config_file`
- `k3s_server_service_env_file`
- `k3s_server_write_kubeconfig_mode`

### Cluster behavior

- `k3s_server_disable_traefik`
- `k3s_server_disable_components`
- `k3s_server_tls_sans`
- `k3s_server_extra_args`

### Networking

- `k3s_server_node_ip`
- `k3s_server_advertise_ip`
- `k3s_server_bind_address`
- `k3s_server_flannel_iface`

## Validation rules

- `k3s_server_cluster_init` must be boolean
- `k3s_server_write_kubeconfig_mode` must be defined
- when `k3s_server_cluster_init` is false, `k3s_server_token` must be defined
- `k3s_server_tls_sans` entries must be non-empty strings
- `k3s_server_extra_args` entries must be non-empty strings

## Notes

This role is intended to run after host foundation and before cluster middleware or application roles.
