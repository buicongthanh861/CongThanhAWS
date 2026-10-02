---
title: "Project overview"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Objective

Build a deployment platform for a three-tier user application on AWS, automating infrastructure, delivery, and operations with DevOps / Cloud Engineering practices.

The application consists of **Frontend**, **Backend**, and **MySQL**. Workloads run on **Amazon EKS**, with container images stored in **Amazon ECR**. Terraform manages infrastructure, while application deployment configuration is versioned in Git.

## Delivery flow

```text
Developer → GitHub Actions → Unit tests → Build image → Push to ECR
                                      → Update Helm values → Argo CD sync → EKS
```

## Technology stack

| Area | Technologies |
|---|---|
| AWS & Infrastructure as Code | AWS EKS, ECR, VPC, IAM, Terraform |
| Containers & GitOps | Kubernetes, Helm, Argo CD |
| CI/CD | GitHub Actions, YQ |
| Security | Trivy, Checkov, GitLeaks |
| Monitoring | Prometheus, Grafana, Alertmanager |

The following sections cover infrastructure design, workload packaging and deployment, CI/CD, security, and monitoring.
