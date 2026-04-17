# gitea_host

Host-level Gitea role installed outside the K3S cluster.

## Responsibility

This role owns:

- host-level Gitea user/group creation
- directory layout
- binary installation
- `app.ini` rendering
- systemd service rendering
- service enablement and verification

This role does not own:

- reverse proxying
- external TLS termination
- K3S installation
- Kubernetes resources

## Required variables

- `gitea_host_domain`
- `gitea_host_root_url`
- `gitea_host_admin_username`
- `gitea_host_admin_password`
- `gitea_host_secret_key`

## Notes

This role is intended to run before K3S and cluster applications.
