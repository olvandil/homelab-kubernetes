# Secrets Management

## Overview

Kubernetes Secrets are used to store sensitive configuration required by platform components and applications.

Examples include:

- Cloudflare API tokens
- GitHub credentials
- Database passwords
- API keys
- TLS certificates

The long-term goal is for all secrets to be managed declaratively alongside the platform while remaining encrypted within Git.

---

## Current Approach

At present, secrets are created manually using `kubectl`.

This provides a simple starting point while the platform is being established.

Examples include:

- `cert-manager-cloudflare-api-token`
- `external-dns-cloudflare-api-token`

Secrets are stored within the namespace of the component that requires them.

For example:

| Namespace | Secret | Purpose |
|-----------|--------|---------|
| `cert-manager` | `cert-manager-cloudflare-api-token` | ACME DNS-01 challenges |
| `external-dns` | `external-dns-cloudflare-api-token` | Cloudflare DNS management |

---

## Design Principles

The platform follows the principle of least privilege.

Each platform component should have:

- Its own API token or credentials.
- Only the permissions required to perform its function.
- Ownership of its own Kubernetes Secrets.

Secrets should never be committed to Git in plain text.

---

## Future Direction

The intended long-term approach is to adopt a GitOps-compatible secrets solution.

Candidate solutions include:

- SOPS
- External Secrets Operator

This will allow encrypted secrets to be stored alongside application and platform manifests while remaining safe to commit to source control.

The desired deployment workflow is:

```text
Git
    │
    ▼
ArgoCD
    │
    ▼
Encrypted Secret
    │
    ▼
Kubernetes Secret
    │
    ▼
Application
```

This ensures a fully declarative deployment process while maintaining the confidentiality of sensitive data.

---

## Future Improvements

- Adopt SOPS for encrypted secrets.
- Evaluate External Secrets Operator for integration with external secret stores.
- Rotate API tokens periodically.
- Audit permissions to ensure least privilege is maintained.
- Eliminate manual secret creation during cluster bootstrap.