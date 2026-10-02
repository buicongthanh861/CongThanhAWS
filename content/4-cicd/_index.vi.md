---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## GitHub Actions pipeline

Workflow `CICD QA Workflow` chạy khi có push vào nhánh `qa` với thay đổi trong `client/`, `server/` hoặc file workflow. Luồng chính:

1. Quét secrets bằng **GitLeaks**; quét Terraform/Kubernetes/Dockerfile bằng **Checkov** và filesystem client/server bằng **Trivy**.
2. Chạy lint/test và build client nếu package có các scripts tương ứng.
3. Build hai image frontend/backend, push lên **Amazon ECR** với tag commit SHA và `latest`.
4. Dùng **YQ** cập nhật image tags trong Helm values ở repository IaC, rồi commit/push lên `master`.
5. **Argo CD** theo dõi thay đổi trên `master` và đồng bộ charts lên EKS.

Các artifact scan được lưu lại trong workflow. Hiện các lệnh Checkov dùng `|| true`, nên kết quả scan không chặn pipeline.

## Kết quả

Quy trình release được báo cáo giảm từ khoảng 30 phút xuống **xấp xỉ 5 phút**. Lưu ý: các package hiện chưa khai báo scripts `lint` hoặc `test`, nên bước gọi với `--if-present` được bỏ qua và workflow chưa thực sự chạy unit tests. Workflow cần AWS credentials, `IAC_REPO_PAT` có quyền push repository IaC và hai ECR repositories frontend/backend.
