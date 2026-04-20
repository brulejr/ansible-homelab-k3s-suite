# gitea_host

Host-level Gitea role installed outside the K3S cluster.

## Responsibility

This role owns:

- host-level Gitea user and group creation
- directory layout
- binary installation
- `app.ini` rendering
- systemd service rendering
- Nginx reverse proxy configuration
- host-level TLS termination with Certbot and Cloudflare DNS challenge
- service enablement and verification

This role does not own:

- K3S installation
- Kubernetes resources
- cluster Traefik ingress
- host-level database backup strategy

## Key variables

### Identity and paths

- `gitea_host_user`
- `gitea_host_group`
- `gitea_host_install_dir`
- `gitea_host_data_dir`
- `gitea_host_config_dir`
- `gitea_host_log_dir`

### Network

- `gitea_host_domain`
- `gitea_host_root_url`
- `gitea_host_http_listen`
- `gitea_host_http_port`
- `gitea_host_ssh_port`

### Database

- `gitea_host_db_type`
- `gitea_host_db_host`
- `gitea_host_db_name`
- `gitea_host_db_user`
- `gitea_host_db_password`

### Bootstrap

- `gitea_host_admin_username`
- `gitea_host_admin_password`
- `gitea_host_admin_email`
- `gitea_host_secret_key`

### TLS / reverse proxy

- `gitea_host_nginx_enabled`
- `gitea_host_tls_enabled`
- `gitea_host_certbot_email`
- `gitea_host_certbot_cloudflare_api_token`

## Validation rules

- `gitea_host_domain` and `gitea_host_root_url` must be defined
- `gitea_host_http_port` and `gitea_host_ssh_port` must be valid TCP ports
- `gitea_host_admin_username`, `gitea_host_admin_password`, and `gitea_host_secret_key` must be defined
- `gitea_host_db_type` must be one of `sqlite3`, `postgres`, or `mysql`
- when `gitea_host_db_type` is not `sqlite3`, `gitea_host_db_host` must be defined
- when Nginx is enabled, `gitea_host_http_listen` must be `127.0.0.1`
- when TLS is enabled, `gitea_host_certbot_email` and `gitea_host_certbot_cloudflare_api_token` must be defined

## Notes

This role is intended to run before K3S and cluster applications.

The intended topology is:

- Gitea listens only on localhost HTTP
- Nginx terminates HTTPS on the host
- Certbot obtains certificates using the Cloudflare DNS plugin
