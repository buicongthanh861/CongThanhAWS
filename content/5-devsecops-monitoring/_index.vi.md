---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Tích hợp kiểm tra bảo mật

Workflow QA có các bước kiểm tra bảo mật:

- **GitLeaks**: quét secrets và tạo artifact báo cáo.
- **Checkov**: quét Terraform, Kubernetes manifests và Dockerfiles.
- **Trivy**: quét filesystem của client/server, mức `HIGH`/`CRITICAL`.

> Lưu ý: Checkov hiện chạy kèm `|| true`, vì vậy phát hiện của công cụ không làm pipeline thất bại.

## Quan sát hệ thống

Project có script port-forward cho **Prometheus** và **Grafana**, theo dõi CPU, memory, pod restarts và trạng thái cluster khi stack monitoring đã được cài.

Hiện resource `kube-prometheus-stack` trong Terraform đang bị comment; Terraform **chưa cài Prometheus/Grafana/Alertmanager tự động**. Cần cài chart `kube-prometheus-stack` vào namespace `monitoring` trước khi dùng các script port-forward.

Email receiver/routing của Alertmanager cần được cấu hình riêng; cấu hình hiện tại chưa chứng minh alert email đã được thiết lập.
