---
title: "Repositories & kết quả"
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## Mã nguồn

1. **Ứng dụng:** [3-tier-user-platform — nhánh `qa`](https://github.com/buicongthanh861/3-tier-user-platform/tree/qa)
2. **Hạ tầng (IaC):** [3-tier-user-platform-iac](https://github.com/buicongthanh861/3-tier-user-platform-iac)

## Kết quả nổi bật

- Tự động hóa provisioning hạ tầng AWS bằng Terraform; thời gian triển khai được báo cáo **dưới 15 phút**.
- Chuẩn hóa triển khai frontend, backend và MySQL trên EKS bằng Helm và GitOps với Argo CD.
- Tự động quét secrets, IaC, Kubernetes manifests, Dockerfiles và filesystem ứng dụng trong workflow QA.
- Build/push image frontend và backend lên ECR; dùng YQ cập nhật GitOps values để Argo CD sync.
- Rút ngắn thời gian release xuống **xấp xỉ 5 phút**.
- Monitoring chart có thể được cài riêng; hiện chưa được Terraform triển khai tự động.

## Việc cần hoàn thiện

- Thêm test/lint scripts để các bước QA hiện có thực sự chạy; hiện `--if-present` sẽ bỏ qua khi chưa có scripts.
- Cân nhắc bỏ `|| true` cho Checkov nếu muốn kết quả policy scan chặn pipeline.
- Cài và cấu hình `kube-prometheus-stack`, bao gồm email receiver của Alertmanager nếu cần cảnh báo email.
- Đồng bộ StorageClass của chart MySQL (`gp3` trong values, `gp2` trong template) và xác nhận Git URL/branch Argo CD (`master`) trước khi triển khai.
