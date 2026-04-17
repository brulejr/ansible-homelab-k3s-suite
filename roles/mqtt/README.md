# mqtt

Self-contained sample MQTT application role for K3S.

## Responsibility

This role owns:

- MQTT namespace manifest
- Mosquitto configmap
- optional persistent volume claim
- MQTT deployment
- MQTT service
- optional Traefik IngressRoute for websockets
- rollout verification

This role does not own:

- K3S installation
- Traefik installation
- shared middleware definitions

## Key variables

### Identity

- `mqtt_namespace`
- `mqtt_app_name`
- `mqtt_hostname`

### Runtime

- `mqtt_image`
- `mqtt_replicas`
- `mqtt_container_port`
- `mqtt_service_port`
- `mqtt_websocket_container_port`
- `mqtt_websocket_port`

### Exposure

- `mqtt_ingress_enabled`
- `mqtt_websocket_enabled`
- `mqtt_ingress_class_name`
- `mqtt_ingress_middlewares`
- `mqtt_tls_enabled`
- `mqtt_tls_cert_resolver`

### Persistence

- `mqtt_persistence_enabled`
- `mqtt_storage_class`
- `mqtt_storage_size`

### Container paths

- `mqtt_config_path`
- `mqtt_data_mount_path`
- `mqtt_log_mount_path`

## Validation rules

- `mqtt_namespace` must be defined
- `mqtt_replicas` must be greater than 0
- `mqtt_service_port` must be a valid TCP port
- when `mqtt_websocket_enabled` is true, `mqtt_websocket_port` must be a valid TCP port
- when `mqtt_ingress_enabled` is true, `mqtt_hostname` must be defined
- when `mqtt_persistence_enabled` is true, `mqtt_storage_class` must be defined

## Notes

This role is intended to be a reusable example of an application role that consumes cluster middleware but does not own it.
