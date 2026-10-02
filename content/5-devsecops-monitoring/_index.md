---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Security checks in CI/CD

The QA workflow includes these security checks:

- **GitLeaks** scans for secrets and creates a report artifact.
- **Checkov** scans Terraform, Kubernetes manifests, and Dockerfiles.
- **Trivy** scans the client/server filesystems for `HIGH`/`CRITICAL` findings.

> Note: Checkov currently runs with `|| true`, so findings do not fail the pipeline.

## System observability

The project includes port-forward scripts for **Prometheus** and **Grafana**, which can expose CPU, memory, pod restart, and cluster status metrics once the monitoring stack is installed.

The `kube-prometheus-stack` resource in Terraform is commented out; Terraform **does not currently install Prometheus/Grafana/Alertmanager automatically**. Install the `kube-prometheus-stack` chart in the `monitoring` namespace before using the port-forward scripts.

Configure an Alertmanager email receiver and routing separately; the current configuration does not establish that email alerts are set up.
