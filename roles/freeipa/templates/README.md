# Role: freeipa_install

Installs and configures FreeIPA server.

## Variables

- `freeipa_realm`: Kerberos realm (e.g., EXAMPLE.LOCAL)
- `freeipa_domain`: DNS domain (e.g., example.local)
- `freeipa_hostname`: Fully qualified domain name.
- `freeipa_admin_password`: Password for the admin user.
- `freeipa_directory_manager_password`: Directory Manager password.

## Example Playbook

```yaml
- hosts: freeipa
  become: true
  roles:
    - role: freeipa_install
```
