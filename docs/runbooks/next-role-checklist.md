# Next Role Checklist

Use this checklist whenever adding a new host role, middleware role, or application role to the homelab.

The goal is to preserve the known-good baseline and introduce change in small, verifiable steps.

## Change policy

For each iteration:

- add only one new role, or one major new behavior, at a time
- update the inventory manifests explicitly
- rerun the full smoke test after the change
- tag both repositories after a successful result

## 1. Classify the role

Decide which class the new role belongs to.

### Host role

Runs directly on the host OS.

Examples:

- host-level databases
- Gitea
- backup agents
- node-local support services

### Cluster bootstrap role

Required to establish the K3S cluster itself.

Examples:

- `k3s_server`
- future `k3s_agent`

### Cluster middleware / platform role

Shared service owned by Kubernetes and consumed by many apps.

Examples:

- Traefik
- storage
- cert-manager
- monitoring
- logging

### Application role

Self-contained deployment for one application.

Examples:

- PlantUML
- MQTT
- future Gitea-in-cluster alternatives
- dashboards
- business apps

## 2. Decide the orchestration layer

Place the role into exactly one manifest list.

### Host foundation

```yaml
host_foundation_roles:
```

### Cluster bootstrap

```yaml
cluster_bootstrap_roles:
```

### Cluster core middleware

```yaml
cluster_core_roles:
```

### Cluster applications

```yaml
cluster_app_roles:
```

Do not put the same role into multiple layers.

## 3. Define the role contract

Before implementation, define the role contract in `defaults/main.yml`.

Use these naming rules:

- every variable is prefixed with the role name
- include `*_enabled`
- group defaults in a predictable order
- avoid hidden dependencies on inventory structure

### Suggested defaults order for application roles

```yaml
role_enabled: true

# Identity
role_namespace:
role_app_name:
role_hostname:

# Image/runtime
role_image:
role_replicas:
role_container_port:
role_service_port:

# Exposure
role_ingress_enabled:
role_ingress_class_name:
role_ingress_middlewares:
role_tls_enabled:
role_tls_cert_resolver:

# Persistence
role_persistence_enabled:
role_storage_class:
role_storage_size:
```

### Suggested defaults order for host/platform roles

- core
- paths
- service/config
- networking
- runtime
- prerequisites

## 4. Implement the standard role shape

Every role should follow this structure:

```text
roles/<role_name>/
├── defaults/main.yml
├── tasks/main.yml
├── tasks/validate.yml
├── tasks/install.yml
├── tasks/verify.yml
├── handlers/main.yml
├── meta/main.yml
├── templates/
└── README.md
```

### `tasks/main.yml`

Should dispatch only:

```yaml
- name: Validate <role>
  ansible.builtin.include_tasks: validate.yml
  when: <role>_enabled | bool

- name: Install and configure <role>
  ansible.builtin.include_tasks: install.yml
  when: <role>_enabled | bool

- name: Verify <role>
  ansible.builtin.include_tasks: verify.yml
  when: <role>_enabled | bool
```

## 5. Add validation rules first

`validate.yml` should fail fast for missing or invalid inputs.

Examples:

- required hostname missing
- invalid port
- persistence enabled without storage class
- dashboard enabled without dashboard hostname
- cluster join mode without required token

Validation should happen before installation work begins.

## 6. Keep ownership boundaries clean

### A role may own

- its own manifests
- its own config
- its own service/deployment/statefulset
- its own PVC
- its own route objects

### A role should not own

- another role’s controller/operator
- shared middleware platform installation
- inventory-only policy decisions
- secrets that belong only in the private repo

Examples:

- app role may consume Traefik, but should not install Traefik
- app role may reference a storage class, but should not install storage middleware

## 7. Add inventory deliberately

When introducing the role:

1. add the role name to exactly one manifest list
2. add required non-secret config to `group_vars/all/main.yml`
3. add required secret config to `group_vars/all/vault.yml`
4. keep defaults usable and reasonable

## 8. Smoke-test expectations

After adding the new role, rerun the full deployment:

```bash
ansible-playbook --ask-vault-pass -i inventories/minisrv/hosts.yml playbooks/site.yml
```

The new role is only accepted when:

- the playbook completes successfully
- the role’s verify step passes
- the previously known-good roles still pass
- the resulting service is reachable or observable as intended

## 9. Post-change validation

Confirm the new role at the right layer.

### Host role

- `systemctl status <service>`
- expected ports listening
- config files present

### Cluster middleware role

- namespace exists
- deployment/statefulset rolls out
- service/controller exists

### Application role

- deployment/statefulset rolls out
- service exists
- route exists if enabled
- expected endpoint is reachable

## 10. Documentation update

For every accepted role addition:

- update the private repo `known-good-baseline.md`
- record any required inventory values
- record whether persistence, ingress, TLS, or storage were enabled
- note any important troubleshooting discoveries

## 11. Tagging policy

After a successful addition:

- tag the suite repo
- tag the inventory repo

This creates a recovery point before the next change.

## 12. Abort criteria

Stop and fix before proceeding if any of these occur:

- a previously working role starts failing
- inventory variables become unclear or duplicated
- a role begins installing shared middleware it does not own
- secrets start creeping into the public suite repo
- multiple unrelated changes are being introduced at once

## 13. Recommended next candidates from the current baseline

Good next additions from the current smoke-tested baseline:

- storage middleware
- monitoring / observability
- backup / restore tooling
- one additional real application role
- optional K3S agent role

## 14. Rule of thumb

If a change makes the system harder to reason about, split it into smaller pieces before merging it.
