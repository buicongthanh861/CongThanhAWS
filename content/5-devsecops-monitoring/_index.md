---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Security checks in CI/CD

Security tools are integrated into CI/CD to identify issues before release:

- **Trivy** scans container images for vulnerabilities.
- **Checkov** checks Terraform and Kubernetes configuration.
- **GitLeaks** detects secrets committed to source code.

## System observability

**Prometheus**, **Grafana**, and **Alertmanager** are used to monitor:

- CPU and memory utilization.
- Pod restarts.
- Kubernetes cluster status and health.

Alertmanager is configured to send email alerts to help surface incidents early.
