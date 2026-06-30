# Prometheus

This application deploys kube-prometheus-stack using Kustomize Helm rendering.

Grafana is disabled because Grafana is managed separately under `applications/grafana`.

## Components

- Prometheus Operator
- Prometheus
- Alertmanager
- node-exporter
- kube-state-metrics
- ServiceMonitor / PodMonitor CRDs
- PrometheusRule CRDs

## Storage

Prometheus and Alertmanager use Longhorn-backed persistent volumes.

## k3s Notes

Some kube-prometheus-stack scrape targets are disabled because k3s packages several control-plane components differently from a standard kubeadm cluster.

Disabled targets:

- kubeEtcd
- kubeControllerManager
- kubeScheduler
- kubeProxy

These can be revisited later once the core monitoring stack is working.