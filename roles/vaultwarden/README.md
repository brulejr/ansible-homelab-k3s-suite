# vaultwarden

Self-contained Vaultwarden application role for K3S.

## Responsibility

This role owns:

- namespace creation
- secret rendering for ADMIN_TOKEN
- optional PVC creation
- deployment rendering and apply
- service rendering and apply
- Traefik IngressRoute rendering and apply
- rollout verification

This role does not own:

- Traefik installation
- Longhorn installation
- SMTP relay provisioning
- backup target configuration

## Required variables

- `vaultwarden_hostname` when ingress is enabled
- `vaultwarden_admin_token`

## Notes

This role assumes TLS terminates at Traefik and Vaultwarden runs plain HTTP inside the cluster.

A persistent volume is recommended for `/data`.

You should pin `vaultwarden_image` in inventory once you choose the version you want to standardize on.
