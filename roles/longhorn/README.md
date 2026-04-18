# longhorn

Cluster-level Longhorn storage middleware role.

## Responsibility

This role owns:

- Longhorn prerequisite package installation
- Longhorn namespace creation
- Longhorn Helm values rendering
- Longhorn Helm installation or upgrade
- optional Longhorn UI IngressRoute
- rollout verification
- storage class verification

This role does not own:

- K3S installation
- Traefik installation
- application persistence configuration
- backup policy design

## Key variables

### Release

- `longhorn_namespace`
- `longhorn_release_name`
- `longhorn_chart_name`
- `longhorn_chart_repo_name`
- `longhorn_chart_repo_url`
- `longhorn_chart_version`

### Runtime

- `longhorn_values_file`
- `longhorn_kubeconfig_path`

### UI exposure

- `longhorn_ui_enabled`
- `longhorn_ui_hostname`
- `longhorn_ingress_class_name`
- `longhorn_ui_middlewares`
- `longhorn_tls_enabled`
- `longhorn_tls_cert_resolver`

### Storage behavior

- `longhorn_default_storage_class`
- `longhorn_storage_class_name`
- `longhorn_default_settings`

## Validation rules

- `longhorn_namespace` must be defined
- when `longhorn_ui_enabled` is true, `longhorn_ui_hostname` must be defined
- `longhorn_storage_class_name` must be defined

## Notes

This role is intended to provide shared persistent storage for cluster applications.
