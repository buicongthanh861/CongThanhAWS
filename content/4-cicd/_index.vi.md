---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## Tổng quan quy trình

GitHub Actions workflow có tên CICD QA Workflow. Workflow được kích hoạt khi có push lên nhánh qa và thay đổi thuộc client, server hoặc chính file workflow. Mục tiêu là kiểm tra mã nguồn, đóng gói hai image ứng dụng, đẩy image lên ECR và cập nhật cấu hình GitOps trong repository IaC.

## Các bước trong pipeline

1. GitLeaks quét repository để phát hiện secrets và lưu báo cáo dạng artifact.
2. Checkov quét Terraform, Kubernetes manifests và Dockerfiles. Kết quả JSON được lưu làm artifact.
3. Trivy quét filesystem của client và server, tập trung vào lỗ hổng mức HIGH và CRITICAL; kết quả quét được lưu lại.
4. Các lệnh lint và test được gọi cho client và server nếu package có khai báo scripts tương ứng.
5. Frontend được build nếu package có build script.
6. Docker build tạo image backend và frontend, sử dụng commit SHA cùng tag latest.
7. AWS credentials được dùng để đăng nhập ECR và push hai image.
8. Job cập nhật repository IaC dùng YQ thay tag image trong k8s/frontend/values.yaml và k8s/backend/values.yaml.
9. Thay đổi Helm values được commit và push lên nhánh master của repository IaC.
10. Argo CD phát hiện commit mới trên master và đồng bộ workload lên EKS.

## Kết nối GitHub Actions với AWS và repository IaC

Workflow cần các GitHub Actions secrets sau:

1. AWS_ACCESS_KEY_ID và AWS_SECRET_ACCESS_KEY để xác thực với AWS.
2. AWS_REGION để chọn region chứa ECR.
3. IAC_REPO_PAT để checkout và push thay đổi vào repository IaC.
4. SONARQUBE_TOKEN chỉ cần nếu bật lại job SonarCloud đang bị comment.

Hai ECR repositories cần tồn tại trước khi push image: 3-tier-user-platform-backend và 3-tier-user-platform-frontend.

## Điều kiện chất lượng hiện tại

Các lệnh lint và test sử dụng tùy chọn if-present. Các package ứng dụng hiện chưa khai báo scripts lint hoặc test, vì vậy GitHub Actions bỏ qua các bước này mà không báo lỗi. Cần bổ sung test runner, test cases và scripts tương ứng trước khi mô tả pipeline là đã xác minh unit tests.

Các lệnh Checkov hiện cho phép workflow tiếp tục ngay cả khi scanner trả về lỗi. Báo cáo scan có thể được lưu để xem xét nhưng hiện chưa đóng vai trò quality gate bắt buộc.

SonarCloud có cấu hình project nhưng job scan đang bị comment. Muốn bật cần cấu hình token, test coverage và bỏ comment job sau khi xác nhận workflow.

## Kết quả

Theo kết quả project cung cấp, thời gian release giảm từ khoảng 30 phút xuống xấp xỉ 5 phút. Pipeline chuẩn hóa đường đi từ commit ứng dụng, tạo image có thể truy vết bằng commit SHA, cập nhật GitOps repository và triển khai qua Argo CD.

Con số thời gian phản ánh kết quả project báo cáo. Cần đo lại trên các lần chạy thực tế để có số liệu benchmark có thể đối chiếu.
