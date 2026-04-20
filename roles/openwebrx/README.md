# openwebrx

OpenWebRX application role for K3S.

## Responsibility

This role owns:

- namespace creation
- optional managed settings.json seed
- PVC creation
- deployment rendering and apply
- service rendering and apply
- Traefik IngressRoute rendering and apply
- rollout verification

This role does not own:

- host RTL-SDR driver installation
- rtl_tcp service setup
- SDR hardware USB access in the cluster

## Notes

This role is designed to pair with the host-level `rtl_sdr_host` role.

By default, it persists `/var/lib/openwebrx` and allows first-boot configuration through the web UI. After you have a known-good receiver definition, you can export `settings.json` and manage it declaratively through `openwebrx_settings_json`.
