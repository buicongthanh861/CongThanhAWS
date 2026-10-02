---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Security in the development workflow

The QA workflow integrates security tools to identify risks in source code, infrastructure configuration, and application artifacts early.

### GitLeaks

GitLeaks scans repository content and history for accidentally committed secrets. The workflow saves the scan report as an artifact for developer review.

### Checkov

Checkov checks Infrastructure as Code and Kubernetes configuration, and scans Dockerfiles. JSON reports are produced for configuration review.

Checkov currently runs in a way that lets the workflow continue when the scanner returns an error. Findings are available for reference, but Checkov does not currently block image build or release.

### Trivy

Trivy scans the frontend and backend filesystems for dependency vulnerabilities. The configuration focuses on HIGH and CRITICAL findings, ignores some vulnerabilities without fixes, and retains scan results as artifacts.

This is a filesystem scan according to the workflow configuration. Do not describe it as scanning images after they are pushed to ECR unless a separate image scan step is configured.

## Monitoring and observability

The IaC repository provides port-forward scripts for Prometheus and Grafana. Once the monitoring stack is installed and its services exist in the monitoring namespace, operators can access local dashboards to observe CPU, memory, Pod status, and Pod restart counts.

The kube-prometheus-stack resource in Terraform is currently commented out. Terraform therefore does not automatically install Prometheus, Grafana, or Alertmanager. Install the monitoring chart separately before running scripts/prometheus.sh or scripts/grafana.sh.

An Alertmanager email receiver and routing have not been confirmed in the configuration provided. To send email alerts, configure a receiver, store SMTP details in a Kubernetes Secret, and define appropriate alert rules.

## Suggested alert handling

1. Identify metrics to monitor, such as CPU, memory, Pod restarts, and workload status.
2. Create alert rules with appropriate thresholds and durations to reduce noisy alerts.
3. Verify that Alertmanager receives alerts from Prometheus.
4. Configure an email receiver and test delivery.
5. Document a response procedure for each alert type.

These steps are recommendations for completion. Describe them as implemented only after the chart is installed and alert delivery has been verified.
