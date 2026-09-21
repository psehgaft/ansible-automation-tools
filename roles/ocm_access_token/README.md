# Role: `ocm_access_token`

Generates a short-lived Red Hat OpenShift Cluster Manager (OCM) access token using a Red Hat Hybrid Cloud Console service account.

## Example

```yaml
---
- name: Generate OCM access token
  hosts: localhost
  gather_facts: false

  roles:
    - role: ocm_access_token
      vars:
        ocm_validate_token: true
```

The role exposes:

```yaml
ocm_access_token
ocm_token_type
ocm_expires_in
ocm_token_generated_at
```

Use `ocm_access_token` as:

```yaml
Authorization: "Bearer {{ ocm_access_token }}"
```
