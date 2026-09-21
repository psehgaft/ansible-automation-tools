# Ansible Role: `ocm_access_token`

This repository contains a reusable Ansible role to generate a short-lived Red Hat OpenShift Cluster Manager (OCM) access token from a Red Hat Hybrid Cloud Console service account.

> OCM access tokens are not non-expiring. Red Hat service-account access grant tokens expire after 15 minutes / 900 seconds. The correct automation model is to store the service account `client_id` and `client_secret` securely, then generate a fresh access token when the playbook runs.

---

## Directory layout

```text
ansible-ocm-token-role/
├── playbooks/
│   └── generate-ocm-token.yml
└── roles/
    └── ocm_access_token/
        ├── defaults/
        │   └── main.yml
        ├── meta/
        │   └── main.yml
        ├── tasks/
        │   ├── assert.yml
        │   ├── main.yml
        │   ├── request_token.yml
        │   └── validate.yml
        └── README.md
```

---

## Required variables

| Variable | Description | Required |
|---|---|---|
| `ocm_client_id` | Red Hat Hybrid Cloud Console service account Client ID | Yes |
| `ocm_client_secret` | Red Hat Hybrid Cloud Console service account Client Secret | Yes |

---

## Optional variables

| Variable | Default | Description |
|---|---|---|
| `ocm_sso_host` | `https://sso.redhat.com` | Red Hat SSO host |
| `ocm_token_endpoint` | `{{ ocm_sso_host }}/auth/realms/redhat-external/protocol/openid-connect/token` | OAuth2 token endpoint |
| `ocm_scopes` | `openid api.iam.service_accounts` | OAuth2 scopes |
| `ocm_api_url` | `https://api.openshift.com` | OCM API base URL |
| `ocm_validate_token` | `false` | Validate the generated token with an OCM API call |
| `ocm_validate_endpoint` | `/api/clusters_mgmt/v1/clusters` | Validation API endpoint |
| `ocm_no_log` | `true` | Hide sensitive token output from logs |

---

## Example: run from environment variables

```bash
export OCM_CLIENT_ID="<client_id>"
export OCM_CLIENT_SECRET="<client_secret>"

ansible-playbook playbooks/generate-ocm-token.yml
```

---

## Example: pass variables explicitly

```bash
ansible-playbook playbooks/generate-ocm-token.yml \
  -e "ocm_client_id=<client_id>" \
  -e "ocm_client_secret=<client_secret>"
```

Avoid passing secrets directly in shell history in production. Prefer Ansible Automation Platform credentials, Ansible Vault, HashiCorp Vault, CyberArk, AWS Secrets Manager, or another enterprise secret manager.

---

## How to use the token in later tasks

The role exposes these facts:

| Fact | Description |
|---|---|
| `ocm_access_token` | Bearer token for OCM API calls |
| `ocm_token_type` | Token type, usually `Bearer` |
| `ocm_expires_in` | Expiration in seconds |
| `ocm_token_generated_at` | Timestamp from the Ansible controller |

Example:

```yaml
- name: Query OCM clusters
  ansible.builtin.uri:
    url: "{{ ocm_api_url }}/api/clusters_mgmt/v1/clusters"
    method: GET
    headers:
      Authorization: "Bearer {{ ocm_access_token }}"
      Accept: "application/json"
    return_content: true
  register: ocm_clusters
  no_log: "{{ ocm_no_log }}"
```

---

## Security recommendations

1. Do not store `ocm_client_secret` in Git.
2. Keep `ocm_no_log: true`.
3. Use AAP credentials or Ansible Vault for the service account secret.
4. Assign least-privilege Red Hat Hybrid Cloud Console roles to the service account.
5. Rotate the service account secret periodically.
6. Regenerate the access token per automation run instead of persisting it.
