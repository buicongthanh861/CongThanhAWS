---
title: "Infrastructure & AWS"
weight: 2
chapter: false
pre: "<b>2. </b>"
---

## Hạ tầng AWS bằng Terraform

Toàn bộ hạ tầng được khai báo bằng **Terraform**, giúp cấu hình có thể kiểm soát phiên bản, xem xét thay đổi trước khi áp dụng và triển khai nhất quán.

- Thiết kế **VPC multi-AZ**, phân tách **Public Subnets** và **Private Subnets**.
- Cấu hình **NAT Gateway**, route và **Security Groups** để kiểm soát luồng mạng.
- Sử dụng CIDR `10.0.0.0/16` tại `ap-southeast-1`, với subnet ở `ap-southeast-1a` và `ap-southeast-1b`.
- Triển khai cluster **staging-demo-eks** và managed node group `general` dùng `t3.medium`; mặc định 2 node, có thể scale tối đa 5 node.
- Cấu hình IAM roles/policies cho EKS, worker nodes, quyền đọc ECR và add-ons; tích hợp OIDC/IRSA.
- Cài AWS Load Balancer Controller và EBS CSI driver; cấu hình StorageClass mặc định.

## Kết quả

Environment mặc định là `staging`, region `ap-southeast-1`; Kubernetes version và một số giá trị môi trường được khai báo trong `terraform/locals.tf`. Thời gian provisioning được báo cáo là **dưới 15 phút**.

> Lưu ý khi vận hành: kiểm tra `terraform plan` trước khi apply; cấu hình hiện dùng một NAT Gateway. Trong chart database, `values.yaml` khai báo `gp3` nhưng StatefulSet template đang cố định `gp2`, cần đồng bộ nếu muốn điều khiển StorageClass qua values.
