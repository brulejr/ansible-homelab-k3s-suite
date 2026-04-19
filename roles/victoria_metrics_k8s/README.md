# victoria_metrics_k8s

Cluster-level VictoriaMetrics K8s Stack monitoring role.

## Responsibility

This role owns:

- monitoring namespace creation
- VictoriaMetrics K8s Stack Helm values rendering
- Helm installation or upgrade
- optional Grafana IngressRoute
- rollout verification

This role does not own:

- K3S installation
- Traefik installation
- log aggregation
- tracing
- alert-routing policy design

## Key variables

### Release

- `victoria_metrics_k8s_namespace`
- `victoria_metrics_k8s_release_name`
- `victoria_metrics_k8s_chart_name`
- `victoria_metrics_k8s_chart_repo_name`
- `victoria_metrics_k8s_chart_repo_url`
- `victoria_metrics_k8s_chart_version`

### Runtime

- `victoria_metrics_k8s_values_file`
- `victoria_metrics_k8s_kubeconfig_path`

### Grafana exposure

- `victoria_metrics_k8s_grafana_enabled`
- `victoria_metrics_k8s_grafana_hostname`
- `victoria_metrics_k8s_ingress_class_name`
- `victoria_metrics_k8s_ui_middlewares`
- `victoria_metrics_k8s_tls_enabled`
- `victoria_metrics_k8s_tls_cert_resolver`

### Persistence

- `victoria_metrics_k8s_persistence_enabled`
- `victoria_metrics_k8s_default_storage_class`
- `victoria_metrics_k8s_vmstorage_size`
- `victoria_metrics_k8s_grafana_size`

## Validation rules

- `victoria_metrics_k8s_namespace` must be defined
- when `victoria_metrics_k8s_grafana_enabled` is true, `victoria_metrics_k8s_grafana_hostname` must be defined
- when `victoria_metrics_k8s_persistence_enabled` is true, `victoria_metrics_k8s_default_storage_class` must be defined

## Notes

This role is intended to provide baseline cluster metrics and dashboards before adding more advanced observability components.
