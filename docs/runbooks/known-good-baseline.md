# Known-Good Baseline

This document captures the first successful end-to-end smoke-test deployment for the private `ansible-homelab-k3s-inventory` repository using the public `ansible-homelab-k3s-suite` submodule.

## Goal

Record the exact baseline that successfully installed and verified:

- host-level Gitea
- K3S server
- Traefik
- PlantUML
- MQTT

This serves as the stable recovery point before adding more middleware or applications.

## Repository relationship

- Public suite repo: `ansible-homelab-k3s-suite`
- Private inventory repo: `ansible-homelab-k3s-inventory`

The private inventory repo owns:

- inventory topology
- manifest selection
- secrets
- environment-specific configuration

The public suite repo owns:

- reusable roles
- layered orchestration playbooks
- example manifest contract

## Successful execution command

From the private inventory repository root:

```bash
ansible-playbook --ask-vault-pass -i inventories/minisrv/hosts.yml playbooks/site.yml
```

## Successful host topology

```yaml
all:
  vars:
    ansible_user: sysadm
  children:
    gitea_hosts:
      hosts:
        minisrv01:
          ansible_host: 10.10.50.50

    k3s_servers:
      hosts:
        minisrv01:
          ansible_host: 10.10.50.50
```

## Successful manifest contract

```yaml
host_foundation_roles:
  - gitea_host

cluster_bootstrap_roles:
  - k3s_server

cluster_core_roles:
  - traefik

cluster_app_roles:
  - plantuml
  - mqtt
```

## Successful environment decisions

### Gitea

- Installed on the host OS, outside K3S
- Managed by systemd
- Uses SQLite for the baseline
- Uses vault-backed admin password and secret key
- Runs on:
  - HTTP port `3000`
  - SSH port `2222`

### K3S

- Installed as a single server node on `minisrv01`
- `k3s_server_cluster_init: true`
- bundled Traefik disabled in K3S so the suite-owned Traefik role is authoritative

### Traefik

- Installed via Helm inside K3S
- Uses explicit K3S kubeconfig path
- Uses ACME configuration from inventory
- Dashboard route enabled

### PlantUML

- Installed as a sample in-cluster application
- Exposed through Traefik
- Persistence disabled for the baseline

### MQTT

- Installed as a sample in-cluster application
- Persistence disabled for the baseline
- Ingress disabled for the baseline
- Websocket ingress disabled for the baseline

## Important inventory values

### Host-level Gitea

```yaml
gitea_host_enabled: true
gitea_host_domain: gitea.lab.brulenet.dev
gitea_host_root_url: "https://gitea.lab.brulenet.dev/"
gitea_host_http_port: 3000
gitea_host_ssh_port: 2222
gitea_host_admin_username: gitea_admin
gitea_host_admin_email: admin@brulenet.dev
gitea_host_admin_password: "{{ vault_gitea_host_admin_password }}"
gitea_host_secret_key: "{{ vault_gitea_host_secret_key }}"
```

### K3S

```yaml
k3s_server_enabled: true
k3s_server_cluster_init: true
k3s_server_disable_traefik: true
k3s_server_tls_sans:
  - minisrv01.lab.brulenet.dev
```

### Traefik

```yaml
traefik_enabled: true
traefik_dashboard_enabled: true
traefik_dashboard_hostname: traefik.lab.brulenet.dev
traefik_acme_enabled: true
traefik_acme_email: admin@brulenet.dev
```

### PlantUML

```yaml
plantuml_enabled: true
plantuml_hostname: plantuml.lab.brulenet.dev
plantuml_persistence_enabled: false
```

### MQTT

```yaml
mqtt_enabled: true
mqtt_hostname: mqtt.lab.brulenet.dev
mqtt_ingress_enabled: false
mqtt_websocket_enabled: false
mqtt_persistence_enabled: false
```

## Vault-backed values

These values are required in the encrypted vault file:

```yaml
vault_gitea_host_admin_password: "<redacted>"
vault_gitea_host_secret_key: "<redacted>"
```

## Validation outcomes

The smoke test is considered successful when all of the following are true:

### Host layer

- Gitea systemd service exists
- Gitea is running
- Gitea HTTP port is listening

### Cluster bootstrap layer

- K3S service exists
- K3S is running
- Kubernetes API is reachable
- `k3s kubectl get nodes` succeeds

### Cluster middleware layer

- Traefik namespace exists
- Traefik deployment rolls out successfully
- Traefik service exists

### Cluster application layer

- PlantUML deployment and service exist
- MQTT deployment and service exist

## Known role fixes incorporated into this baseline

This baseline includes the following corrections made during smoke testing:

### Gitea

- `app.ini` must be writable by the `git` runtime user
- host verification uses service facts and localhost port checks
- loop-variable collision cleaned up in install tasks

### K3S

- install flow split into:
  - install by channel
  - install by explicit version
- avoid passing the Ansible omit placeholder into `INSTALL_K3S_VERSION`
- verify task cleaned up to avoid Jinja delimiter warnings

### Traefik

- Helm is installed directly from the release archive rather than relying on distro package availability
- Helm commands use explicit K3S kubeconfig
- values template updated to match the current Traefik chart schema

### PlantUML

- normalized on `plantuml_persistence_enabled`

### MQTT

- normalized on `mqtt_persistence_enabled`
- persistence disabled for the baseline to avoid requiring storage configuration at first smoke test

## Next-step policy

From this point forward:

- add only one new role or one new major behavior at a time
- rerun the full smoke test after each significant addition
- tag both repositories after each known-good milestone
- document any inventory changes required to support the new role

## Recommended next candidates

Reasonable next additions from this baseline include:

- storage middleware
- cert-manager, if desired separately from Traefik ACME
- a first “real” app beyond the two sample apps
- monitoring/observability
- backup/restore workflows

## Recovery guidance

If future changes break the environment:

1. return both repositories to this known-good tag
2. rerun the smoke-test command
3. confirm the baseline succeeds again
4. reintroduce later changes incrementally
