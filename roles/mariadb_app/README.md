# mariadb_app

Reusable MariaDB application role for cluster apps that need a database.

## Responsibility

This role owns:

- namespace creation
- secret creation for database credentials
- PVC creation
- deployment rendering and apply
- service rendering and apply
- rollout verification

This role does not own:

- database schema migrations
- application installers
- ingress exposure
