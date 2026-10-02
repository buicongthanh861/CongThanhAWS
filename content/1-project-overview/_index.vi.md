---
title: "Giới thiệu dự án"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Mục tiêu

Xây dựng nền tảng triển khai ứng dụng người dùng theo kiến trúc ba tầng trên AWS, đồng thời tự động hóa hạ tầng, phát hành và vận hành theo các thực hành DevOps / Cloud Engineering.

Ứng dụng gồm **Frontend**, **Backend** và **MySQL**. Các workload chạy trên **Amazon EKS**; image được lưu trữ trên **Amazon ECR**. Cấu hình hạ tầng được quản lý bằng Terraform và cấu hình triển khai ứng dụng được quản lý trong Git.

## Luồng triển khai

```text
Developer → GitHub Actions → Unit tests → Build image → Push to ECR
                                      → Update Helm values → Argo CD sync → EKS
```

## Công nghệ sử dụng

| Nhóm | Công nghệ |
|---|---|
| AWS & Infrastructure as Code | AWS EKS, ECR, VPC, IAM, Terraform |
| Container & GitOps | Kubernetes, Helm, Argo CD |
| CI/CD | GitHub Actions, YQ |
| Bảo mật | Trivy, Checkov, GitLeaks |
| Monitoring | Prometheus, Grafana, Alertmanager |

Các phần tiếp theo trình bày thiết kế hạ tầng, cách đóng gói và triển khai workload, pipeline CI/CD, bảo mật và giám sát.
