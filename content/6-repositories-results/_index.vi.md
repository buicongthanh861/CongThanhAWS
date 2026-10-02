---
title: "Repositories & kết quả"
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## Repository dự án

1. Ứng dụng: [3-tier-user-platform, nhánh qa](https://github.com/buicongthanh861/3-tier-user-platform/tree/qa)
2. Hạ tầng Infrastructure as Code: [3-tier-user-platform-iac](https://github.com/buicongthanh861/3-tier-user-platform-iac)

## Kết quả đạt được

1. Xây dựng ứng dụng quản lý người dùng ba tầng với giao diện React, REST API Node.js và cơ sở dữ liệu MySQL.
2. Khai báo hạ tầng mạng AWS và Amazon EKS bằng Terraform, hỗ trợ triển khai lại môi trường từ cấu hình trong repository.
3. Tổ chức frontend, backend và MySQL thành Helm charts riêng, triển khai trên các namespace riêng trong Kubernetes.
4. Tự động đồng bộ cấu hình từ repository IaC tới EKS bằng Argo CD với automated sync, prune và self-heal.
5. Tự động build và push image frontend, backend lên ECR, dùng commit SHA để truy vết phiên bản.
6. Cập nhật image tags trong Helm values bằng YQ và kích hoạt luồng triển khai GitOps.
7. Thời gian provisioning được báo cáo dưới 15 phút và thời gian release được báo cáo khoảng 5 phút.

Các số liệu về thời gian là kết quả project cung cấp. Nên lưu lại log thời gian chạy thực tế nếu cần dùng làm số liệu đo lường trong hồ sơ.

## Giới hạn hiện tại

1. Frontend mới nối chức năng xem danh sách và tạo người dùng. Các thao tác cập nhật và xóa đã có ở backend nhưng chưa hoàn thiện trên giao diện.
2. Các package chưa có scripts lint và test. Những bước gọi bằng if-present hiện được bỏ qua.
3. Checkov hiện không chặn pipeline khi phát hiện lỗi do cấu hình cho phép tiếp tục.
4. Monitoring stack chưa được Terraform cài tự động; cấu hình gửi email của Alertmanager cần được bổ sung và kiểm thử.
5. Template MySQL đang cố định gp2 trong khi values khai báo gp3.
6. Cần xác minh repository URL và nhánh master trong Argo CD Application manifests trước khi triển khai.
7. Cấu hình demo cần được rà soát về credentials, quyền IAM, điểm truy cập EKS, TLS và exposure của ingress trước khi dùng production.

## Hướng phát triển

1. Bổ sung unit tests và lint scripts cho frontend và backend, sau đó cấu hình pipeline để các bước này bắt buộc thành công.
2. Cấu hình Checkov thành quality gate sau khi rà soát và xử lý các phát hiện phù hợp.
3. Cài kube-prometheus-stack, tạo dashboard và alert rules, cấu hình email receiver cho Alertmanager.
4. Đồng bộ StorageClass của MySQL chart và kiểm thử quá trình tạo PVC trên EKS.
5. Hoàn thiện thao tác sửa và xóa người dùng trên giao diện.
6. Bổ sung xác thực, phân quyền, kiểm tra dữ liệu đầu vào và giới hạn CORS theo môi trường.
7. Cấu hình Terraform remote state có encryption và locking, đồng thời kiểm tra backup và khôi phục dữ liệu MySQL.

## Tổng kết

Project kết hợp phát triển ứng dụng, Infrastructure as Code, container, Kubernetes, CI/CD, GitOps và các bước kiểm tra bảo mật. Kiến trúc tách riêng repository ứng dụng và repository hạ tầng, giúp thay đổi image được đưa qua Git để xem xét và đồng bộ có kiểm soát tới EKS.
