# plantuml

Self-contained sample PlantUML application role for K3S.

## Responsibility

This role owns:

- PlantUML namespace manifest
- PlantUML deployment
- PlantUML service
- optional persistent volume claim
- optional Traefik IngressRoute
- rollout verification

This role does not own:

- K3S installation
- Traefik installation
- shared middleware definitions

## Key variables

### Identity

- `plantuml_namespace`
- `plantuml_app_name`
- `plantuml_hostname`

### Runtime

- `plantuml_image`
- `plantuml_replicas`
- `plantuml_container_port`
- `plantuml_service_port`

### Exposure

- `plantuml_ingress_enabled`
- `plantuml_ingress_class_name`
- `plantuml_ingress_middlewares`
- `plantuml_tls_enabled`
- `plantuml_tls_cert_resolver`

### Persistence

- `plantuml_persistence_enabled`
- `plantuml_storage_class`
- `plantuml_storage_size`

## Validation rules

- `plantuml_namespace` must be defined
- `plantuml_service_port` must be a valid TCP port
- `plantuml_replicas` must be greater than 0
- when `plantuml_ingress_enabled` is true, `plantuml_hostname` must be defined
- when `plantuml_persistence_enabled` is true, `plantuml_storage_class` must be defined

## Notes

This role is intended to be a reusable example of an application role that consumes cluster middleware but does not own it.
