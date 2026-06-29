# DNS & Ingress

## Overview

The homelab uses separate solutions for public and internal DNS to allow a gradual migration from the existing infrastructure.

Public DNS is managed automatically by ExternalDNS and Cloudflare.

Internal DNS is currently managed manually by Unbound on OPNsense.

This allows existing services hosted behind Nginx Proxy Manager to continue operating while new services are migrated to Kubernetes.

---

## Public DNS

Public DNS is hosted by Cloudflare.

ExternalDNS monitors Kubernetes Ingress resources and automatically creates, updates and removes DNS records.

```
Ingress
    │
    ▼
ExternalDNS
    │
    ▼
Cloudflare
```

### Managed Domain

- `watworld.uk`

---

## Internal DNS

Internal DNS is provided by Unbound running on OPNsense.

Static overrides are currently used to direct Kubernetes applications to the Traefik ingress controller.

Example:

| Hostname | Destination |
|----------|-------------|
| argocd.watworld.uk | Traefik |
| nginx-test.watworld.uk | Traefik |

Existing applications continue to resolve to Nginx Proxy Manager until they are migrated.

---

## Ingress

Traefik is the cluster ingress controller.

It is responsible for routing incoming HTTP(S) requests to Kubernetes Services based on hostname.

Example flow:

```
Client
    │
    ▼
Traefik
    │
    ▼
Ingress
    │
    ▼
Service
    │
    ▼
Pods
```

---

## TLS

TLS certificates are managed by cert-manager using a Cloudflare DNS-01 ClusterIssuer.

Certificates are requested automatically when a Certificate resource is deployed.

---

## Migration Strategy

The migration from the existing infrastructure is intentionally gradual.

Current production services continue to use Nginx Proxy Manager.

New Kubernetes applications are migrated individually by:

1. Deploying the application to Kubernetes.
2. Creating an internal Unbound override pointing to Traefik.
3. Verifying functionality.
4. Allowing ExternalDNS to manage the public Cloudflare record.
5. Retiring the previous Nginx Proxy Manager configuration.

This approach minimises disruption while allowing both platforms to coexist during the migration.

---

## Future Improvements

- Automate Kubernetes secrets using SOPS or External Secrets Operator.
- Investigate automation of internal DNS.
- Introduce a highly available ingress endpoint using a dedicated virtual IP.
- Remove Nginx Proxy Manager once all applications have migrated to Kubernetes.