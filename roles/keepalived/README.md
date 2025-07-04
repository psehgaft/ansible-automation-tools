# Role: keepalived_install

This role installs and configures Keepalived for high availability on Fedora/CentOS/RHEL systems.

## Role Variables

- `keepalived_package`: Package name.
- `keepalived_service`: Service name.
- `keepalived_config_path`: Configuration file path.
- `keepalived_interface`: Network interface for VRRP.
- `keepalived_virtual_ip`: VIP address.
- `keepalived_priority`: Priority in VRRP election.
- `keepalived_router_id`: Identifier for the router.

## Example Playbook

```yaml
- hosts: ha_nodes
  become: true
  roles:
    - role: keepalived_install
```
