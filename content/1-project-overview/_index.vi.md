---
title: "Giới thiệu dự án"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Mục tiêu

Triển khai ứng dụng quản lý người dùng theo kiến trúc ba tầng trên AWS, đồng thời tự động hóa hạ tầng, phát hành và vận hành theo các thực hành DevOps / Cloud Engineering.

Ứng dụng **3-Tier User Platform** gồm frontend **React**, backend **Node.js/Express** cung cấp REST API quản lý người dùng và cơ sở dữ liệu **MySQL 8**. Ba thành phần được đóng gói riêng và triển khai trên **Amazon EKS**; image frontend/backend được lưu ở **Amazon ECR**. Terraform quản lý AWS infrastructure và add-ons; Argo CD theo dõi các Helm charts trong repository IaC.

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

## Luồng CI/CD và GitOps

```text
Push vào nhánh qa → GitLeaks / Checkov / Trivy → lint, test nếu có scripts
                 → Build image → Push ECR → YQ cập nhật Helm values ở repo IaC
                 → Argo CD theo dõi nhánh master → Sync lên EKS
```

## Công nghệ sử dụng

| Nhóm | Công nghệ |
|---|---|
| AWS & Infrastructure as Code | AWS EKS, ECR, VPC, IAM, Terraform |
| Container & GitOps | Kubernetes, Helm, Argo CD |
| CI/CD | GitHub Actions, YQ |
| Bảo mật | Trivy, Checkov, GitLeaks |
| Monitoring | Prometheus, Grafana, Alertmanager |

> Workflow hiện gọi lint/test bằng `--if-present`; các scripts lint/test chưa được khai báo trong package hiện tại nên các bước đó được bỏ qua. Add-on kube-prometheus-stack cũng chưa được Terraform cài tự động.

Các phần tiếp theo trình bày hạ tầng, chart Kubernetes, luồng CI/CD thực tế, các kiểm tra bảo mật và trạng thái monitoring.
