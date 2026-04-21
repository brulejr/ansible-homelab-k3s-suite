# wavelog

Wavelog application role for K3S.

## Responsibility

This role owns:

- namespace creation
- app secret creation
- PVC creation
- deployment rendering and apply
- service rendering and apply
- Traefik ingressroute rendering and apply
- rollout verification

This role does not own:

- MariaDB lifecycle
- Wavelog installer workflow
- station and callsign setup inside the app
