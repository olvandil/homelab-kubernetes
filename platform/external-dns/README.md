# ExternalDNS

## Purpose

ExternalDNS automatically manages public DNS records in Cloudflare based on Kubernetes resources.

When an Ingress is created, updated, or removed, ExternalDNS ensures the corresponding DNS record in Cloudflare is created, updated, or deleted.

This removes the need to manually manage DNS records when deploying applications.

## Configuration

- **Provider:** Cloudflare  
- **Managed domain:** `watworld.uk`  
- **Source:** Kubernetes Ingress resources  
- **Registry:** TXT  
- **TXT Owner ID:** `homelab-k3s`  

## Current Scope

ExternalDNS currently manages **public DNS records in Cloudflare only**.

Internal DNS (Unbound on OPNsense) remains manually managed. This is intentional to support a safe, incremental migration from existing Nginx Proxy Manager-based routing.

## Notes

- ExternalDNS will only create records for Kubernetes resources that expose hostnames (e.g. Ingress objects).
- It does not handle TLS or routing—only DNS record management.

## Future Improvements

- Migrate Cloudflare API token to a GitOps-friendly secret management approach (e.g. SOPS or External Secrets Operator).
- Gradually migrate more services from Nginx Proxy Manager to Kubernetes + Traefik.
- Re-evaluate internal DNS automation once full service migration is complete.