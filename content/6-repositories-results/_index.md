---
title: "Repositories & outcomes"
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## Source code

1. **Application:** [3-tier-user-platform — `qa` branch](https://github.com/buicongthanh861/3-tier-user-platform/tree/qa)
2. **Infrastructure (IaC):** [3-tier-user-platform-iac](https://github.com/buicongthanh861/3-tier-user-platform-iac)

## Highlights

- Automated AWS infrastructure provisioning with Terraform, with reported deployment time of **under 15 minutes**.
- Standardized frontend, backend, and MySQL deployment on EKS with Helm and Argo CD GitOps.
- Automated secret, IaC, Kubernetes manifest, Dockerfile, and application filesystem scans in the QA workflow.
- Built/pushed frontend and backend images to ECR and used YQ to update GitOps values for Argo CD sync.
- Reduced release time to **approximately 5 minutes**.
- Monitoring can be installed separately; Terraform does not currently deploy the chart automatically.

## Follow-up improvements

- Add test/lint scripts so the existing QA steps actually run; `--if-present` skips them when scripts are absent.
- Consider removing `|| true` from Checkov if policy findings should block the pipeline.
- Install and configure `kube-prometheus-stack`, including an Alertmanager email receiver if email notifications are required.
- Align the MySQL chart StorageClass (`gp3` in values, `gp2` in the template) and verify the Argo CD Git URL/branch (`master`) before deployment.
