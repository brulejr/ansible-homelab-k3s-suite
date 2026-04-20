# nodered

Node-RED application role for K3S.

## Responsibility

This role owns:

- namespace creation
- `settings.js` configmap rendering
- optional PVC creation
- deployment rendering and apply
- service rendering and apply
- Traefik IngressRoute rendering and apply
- rollout verification

This role does not own:

- Traefik installation
- Longhorn installation
- external authentication provider setup
- palette module lifecycle beyond base runtime config

## Required variables

- `nodered_hostname` when ingress is enabled

## Notes

This role mounts `/data` persistently, which aligns with Node-RED’s Docker guidance. It also enables file-backed context storage by default so context can survive restarts.

Admin authentication is optional. If enabled, provide a pre-generated bcrypt hash via inventory.
