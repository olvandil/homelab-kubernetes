# Homelab Kubernetes

This repository contains the Kubernetes configuration for my homelab.

## Goals

- GitOps using ArgoCD
- Automated TLS using cert-manager and Cloudflare
- Platform services managed declaratively
- Applications deployed from Git

## Repository Layout

bootstrap/
    Components required before GitOps.

platform/
    Cluster-wide platform services.

applications/
    Workloads deployed to the cluster.

environments/
    Environment-specific configuration.

docs/
    Design notes and documentation.