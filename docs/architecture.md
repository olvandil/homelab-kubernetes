# Homelab Kubernetes Architecture

## Overview

This repository defines the desired state of the Kubernetes platform and applications running in the homelab.

The cluster follows a GitOps model using ArgoCD.

## Responsibility Split

| Area | Tool |
|---|---|
| Infrastructure provisioning | Terraform |
| Kubernetes cluster | k3s |
| GitOps reconciliation | ArgoCD |
| Ingress routing | Traefik |
| TLS certificates | cert-manager |
| Public DNS automation | ExternalDNS |
| DNS provider | Cloudflare |
| Internal DNS | OPNsense Unbound |

## Repositories

| Repository | Purpose |
|---|---|
| `homelab-terraform` | Proxmox VM provisioning and infrastructure lifecycle |
| `homelab-kubernetes` | Kubernetes platform and application configuration |
| `homelab-ansible` | Future OS and k3s bootstrap automation |

## GitOps Flow

```text
GitHub
  ↓
ArgoCD
  ↓
Kubernetes
  ↓
Traefik / cert-manager / ExternalDNS