# Role: haproxy_install

This role installs and configures HAProxy on Fedora/CentOS/RHEL systems.

## Role Variables

- `haproxy_package`: Name of the package to install.
- `haproxy_service`: Service name.
- `haproxy_config_path`: Path to the configuration file.
- `haproxy_listen_port`: Frontend port.
- `haproxy_backend_servers`: List of backend servers.

## Example Playbook

```yaml
- hosts: balancers
  become: true
  roles:
    - role: haproxy_install
```
