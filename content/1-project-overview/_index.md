---
title: "Project overview"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Objective

Deploy a three-tier user-management application on AWS while automating infrastructure, delivery, and operations with DevOps / Cloud Engineering practices.

**3-Tier User Platform** consists of a **React frontend**, a **Node.js/Express REST API** for user management, and **MySQL 8**. The three components are packaged separately and deployed on **Amazon EKS**; frontend and backend images are stored in **Amazon ECR**. Terraform manages AWS infrastructure and add-ons; Argo CD tracks the Helm charts in the IaC repository.

{{< mermaid >}}
flowchart TB
    Internet --> Ingress[NGINX Ingress]
    Ingress --> Frontend[Frontend Service]
    Ingress --> Backend[Backend Service /api]
    Backend --> Database[MySQL StatefulSet]
    Terraform[Terraform] --> VPC[VPC, subnets, routes, NAT]
    Terraform --> EKS[EKS cluster and node group]
    Terraform --> Addons[OIDC, AWS Load Balancer Controller, EBS CSI, Argo CD]
    ArgoCD[Argo CD] --> Frontend
    ArgoCD --> Backend
    ArgoCD --> Database
{{< /mermaid >}}

## CI/CD and GitOps flow

```text
Push to qa → GitLeaks / Checkov / Trivy → lint and test if scripts exist
          → Build images → Push to ECR → YQ updates Helm values in IaC repo
          → Argo CD tracks master → Sync to EKS
```

## Technology stack

| Area | Technologies |
|---|---|
| AWS & Infrastructure as Code | AWS EKS, ECR, VPC, IAM, Terraform |
| Containers & GitOps | Kubernetes, Helm, Argo CD |
| CI/CD | GitHub Actions, YQ |
| Security | Trivy, Checkov, GitLeaks |
| Monitoring | Prometheus, Grafana, Alertmanager |

> The workflow invokes lint/test with `--if-present`; those scripts are not currently defined in the packages, so the steps are skipped. The kube-prometheus-stack add-on is also not installed automatically by Terraform.

The following sections cover infrastructure, Kubernetes charts, the actual CI/CD flow, security checks, and monitoring status.
