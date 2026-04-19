# traefik

Cluster-level Traefik ingress and TLS termination role.

## Responsibility

This role owns:

- Traefik namespace creation
- Traefik Helm values rendering
- Traefik Helm installation or upgrade
- Cloudflare credential secret creation for ACME DNS challenge
- optional dashboard ingress route
- basic rollout verification

This role does not own:

- K3S installation
- application deployments
- host-level Gitea TLS termination
- per-application ingress definitions outside Traefik CRDs

## Required variables

- `traefik_namespace`
- `traefik_ingress_class_name`
- `traefik_service_type`

## ACME notes

When `traefik_acme_enabled` is true:

- `traefik_acme_email` must be defined
- `traefik_acme_resolver_name` should be set

When `traefik_acme_dns_challenge_enabled` is true:

- `traefik_acme_dns_provider` must be defined
- if using Cloudflare, `traefik_cloudflare_dns_api_token` must be supplied from the private inventory

## Notes

This role is intended to handle TLS for cluster apps through Traefik.

Host-level services such as `gitea_host` should use a separate TLS strategy.
