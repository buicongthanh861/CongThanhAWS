---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## GitHub Actions pipeline

Pipeline tự động hóa quy trình kiểm thử và phát hành ứng dụng theo thứ tự:

1. Chạy **Unit Tests**.
2. Build container image.
3. Push image lên **Amazon ECR**.
4. Dùng **YQ** cập nhật image tag trong Helm values.
5. **Argo CD** phát hiện thay đổi trong Git và đồng bộ lên cluster.

## Kết quả

Quy trình release được tự động hóa, giảm thời gian phát hành từ khoảng 30 phút xuống còn **xấp xỉ 5 phút** theo kết quả project cung cấp. Việc cập nhật tag qua Git cũng tạo lịch sử thay đổi rõ ràng và kích hoạt luồng GitOps.
