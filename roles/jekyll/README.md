# jekyll

Cluster-side Jekyll application role for K3S.

## Responsibility

This role owns:

- namespace creation
- git SSH secret creation
- bundle cache PVC creation
- deployment rendering and apply
- service rendering and apply
- Traefik ingressroute rendering and apply
- rollout verification

This role does not own:

- Gitea user creation
- repo bootstrap/seeding
- deploy key generation on the host
