---
title: "DevOps / Cloud Engineer"
weight: 1
chapter: false
---
# 3-Tier User Platform trên AWS

Xin chào, tôi là **buicongthanh861**. Đây là báo cáo project **3-Tier User Platform**, trình bày cách triển khai ứng dụng quản lý người dùng trên AWS và tự động hóa hạ tầng, CI/CD, GitOps.

## Tổng quan dự án

Ứng dụng gồm giao diện React, REST API dùng Node.js/Express và cơ sở dữ liệu MySQL. Hạ tầng Kubernetes được triển khai trên Amazon EKS bằng Terraform. GitHub Actions kiểm tra mã nguồn, build/push image lên ECR và cập nhật Helm values trong repository IaC; Argo CD đồng bộ các thay đổi lên cluster.

**Tech stack:** AWS EKS, ECR, VPC, IAM, Terraform, Kubernetes, Helm, Argo CD, GitHub Actions, Prometheus, Grafana, Trivy, Checkov, GitLeaks.

## Nội dung

1. [Giới thiệu dự án](1-project-overview/)
2. [Infrastructure & AWS](2-infrastructure-aws/)
3. [Kubernetes & GitOps](3-kubernetes-gitops/)
4. [CI/CD](4-cicd/)
5. [DevSecOps & Monitoring](5-devsecops-monitoring/)
6. [Repositories & kết quả](6-repositories-results/)
