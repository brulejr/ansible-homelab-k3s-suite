# plantuml

Self-contained sample PlantUML application role for K3S.

## Responsibility

This role owns:

- PlantUML namespace manifest
- PlantUML deployment
- PlantUML service
- optional PVC
- optional Traefik IngressRoute
- rollout verification

This role does not own:

- K3S installation
- Traefik installation
- shared middleware definitions

## Required variables

- `plantuml_namespace`
- `plantuml_app_name`
- `plantuml_service_port`

## Notes

When `plantuml_ingress_enabled` is true, `plantuml_hostname` must be defined.
When `plantuml_storage_enabled` is true, `plantuml_storage_class` must be defined.
