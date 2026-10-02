---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Tích hợp kiểm tra bảo mật

Các công cụ bảo mật được đưa vào CI/CD để phát hiện sớm vấn đề trước khi phát hành:

- **Trivy**: quét lỗ hổng container image.
- **Checkov**: kiểm tra cấu hình Terraform và Kubernetes.
- **GitLeaks**: phát hiện secrets bị đưa vào mã nguồn.

## Quan sát hệ thống

Triển khai **Prometheus**, **Grafana** và **Alertmanager** để theo dõi:

- Mức sử dụng CPU và memory.
- Pod restarts.
- Trạng thái và sức khỏe của Kubernetes cluster.

Alertmanager được cấu hình gửi cảnh báo qua email để hỗ trợ phát hiện sự cố sớm.
