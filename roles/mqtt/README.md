# mqtt

Self-contained sample MQTT application role for K3S.

## Responsibility

This role owns:

- MQTT namespace manifest
- Mosquitto configmap
- optional PVC
- MQTT deployment
- MQTT service
- optional Traefik IngressRoute for websockets
- rollout verification

This role does not own:

- K3S installation
- Traefik installation
- shared middleware definitions

## Required variables

- `mqtt_namespace`
- `mqtt_app_name`
- `mqtt_service_port`

## Notes

When `mqtt_ingress_enabled` is true, `mqtt_hostname` must be defined.
When `mqtt_persistence_enabled` is true, `mqtt_storage_class` must be defined.
