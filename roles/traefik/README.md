# traefik

Cluster-level Traefik ingress and TLS termination role.

## Responsibility

This role owns:

- Traefik namespace creation
- Traefik Helm values rendering
- Traefik Helm installation or upgrade
- optional dashboard ingress route
- basic rollout verification

This role does not own:

- K3S installation
- application deployments
- per-application ingress definitions

## Required variables

- `traefik_namespace`
- `traefik_ingress_class_name`
- `traefik_service_type`

## Notes

When `traefik_acme_enabled` is true, `traefik_acme_email` must be defined.
When `traefik_dashboard_enabled` is true, `traefik_dashboard_hostname` must be defined.
