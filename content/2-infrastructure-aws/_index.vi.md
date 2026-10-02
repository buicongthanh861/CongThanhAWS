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
- Triển khai **Amazon EKS** cùng các **IAM Roles** cần thiết cho cluster và workload.
- Tổ chức Terraform thành các **module tái sử dụng**, chuẩn hóa cách khởi tạo môi trường.

## Kết quả

Thời gian provisioning được rút ngắn từ vài giờ xuống **dưới 15 phút** theo kết quả project cung cấp. Hạ tầng có thể được triển khai lặp lại từ cấu hình đã quản lý trong repository IaC.

> Các giá trị hiệu năng được trình bày theo kết quả của project; thời gian thực tế có thể thay đổi theo cấu hình AWS và trạng thái dịch vụ.
