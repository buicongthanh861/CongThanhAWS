---
title: "DevOps / Cloud Engineer"
weight: 1
chapter: false
---
# Nền tảng người dùng 3-Tier trên AWS

Xin chào, tôi là **buicongthanh861**. Đây là báo cáo project DevOps / Cloud Engineer trình bày quá trình xây dựng và tự động hóa một nền tảng ứng dụng ba tầng trên AWS.

## Tổng quan dự án

Dự án triển khai frontend, backend và cơ sở dữ liệu MySQL trên Kubernetes (Amazon EKS). Hạ tầng được định nghĩa bằng Terraform; quy trình phát hành sử dụng GitHub Actions và Argo CD theo mô hình GitOps.

**Tech stack:** AWS EKS, ECR, VPC, IAM, Terraform, Kubernetes, Helm, Argo CD, GitHub Actions, Prometheus, Grafana, Trivy, Checkov, GitLeaks.

## Nội dung

1. [Giới thiệu dự án](1-project-overview/)
2. [Infrastructure & AWS](2-infrastructure-aws/)
3. [Kubernetes & GitOps](3-kubernetes-gitops/)
4. [CI/CD](4-cicd/)
5. [DevSecOps & Monitoring](5-devsecops-monitoring/)
6. [Repositories & kết quả](6-repositories-results/)
