# rtl_sdr_host

Host-level RTL-SDR role that installs RTL-SDR userspace support and runs rtl_tcp.

## Responsibility

This role owns:

- installing build prerequisites
- cloning and building RTL-SDR userspace
- installing udev rules
- blacklisting DVB kernel drivers
- rendering and managing rtl_tcp as a systemd service

This role does not own:

- OpenWebRX application deployment
- Traefik ingress
- SDR band/profile configuration inside OpenWebRX

## Notes

This role is intended to keep direct USB SDR access on the host while exposing the receiver over the network with rtl_tcp.
