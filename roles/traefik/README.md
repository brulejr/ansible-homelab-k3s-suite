# traefik

Cluster-level Traefik ingress and TLS termination role.

## Responsibility

This role owns:

- Traefik namespace creation
- Traefik Helm values rendering
- Traefik Helm installation or upgrade
- optional dashboard IngressRoute
- rollout verification

This role does not own:

- K3S installation
- application deployments
- per-application ingress definitions

## Key variables

### Release

- `traefik_namespace`
- `traefik_release_name`
- `traefik_chart_name`
- `traefik_chart_repo_name`
- `traefik_chart_repo_url`
- `traefik_chart_version`

### Service and ingress

- `traefik_ingress_class_name`
- `traefik_service_type`

### Dashboard

- `traefik_dashboard_enabled`
- `traefik_dashboard_hostname`

### ACME / TLS

- `traefik_acme_enabled`
- `traefik_acme_email`
- `traefik_acme_ca_server`
- `traefik_acme_storage_path`

## Validation rules

- `traefik_namespace` must be defined
- when `traefik_acme_enabled` is true, `traefik_acme_email` must be defined
- when `traefik_dashboard_enabled` is true, `traefik_dashboard_hostname` must be defined

## Notes

This role is intended to provide shared ingress and TLS capabilities for cluster applications.
