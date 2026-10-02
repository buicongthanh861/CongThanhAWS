---
title: "Giới thiệu dự án"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Mục tiêu dự án

3-Tier User Platform là ứng dụng quản lý người dùng được triển khai theo kiến trúc ba tầng. Mục tiêu của project là đóng gói ứng dụng, tự động hóa hạ tầng AWS và xây dựng quy trình phát hành có thể lặp lại bằng CI/CD và GitOps.

Phần ứng dụng gồm giao diện React, REST API dùng Node.js và Express, cùng cơ sở dữ liệu MySQL. Phần hạ tầng sử dụng Terraform để tạo mạng và Amazon EKS. Kubernetes workloads được mô tả bằng Helm charts; GitHub Actions build và phát hành image, còn Argo CD đồng bộ cấu hình triển khai từ Git.

## Kiến trúc ứng dụng

### Tầng giao diện

Frontend được xây dựng bằng React 17, React DOM, Axios và Webpack 5. Người dùng có thể xem danh sách người dùng và gửi thông tin người dùng mới qua giao diện. Frontend gọi REST API qua đường dẫn API của ứng dụng.

Image frontend được build từ Dockerfile riêng và được phục vụ bằng Nginx. Image sau khi build được đẩy lên Amazon ECR để Kubernetes tải về khi triển khai.

### Tầng xử lý nghiệp vụ

Backend chạy trên Node.js 18 trở lên, sử dụng Express 4, MySQL2 và CORS. API lắng nghe mặc định trên cổng 5000 và cung cấp các thao tác:

1. GET /api/users để đọc danh sách người dùng.
2. POST /api/users để tạo người dùng.
3. PUT /api/users/:id để cập nhật người dùng.
4. DELETE /api/users/:id để xóa người dùng.

Backend nhận cấu hình kết nối cơ sở dữ liệu qua biến môi trường. Khi khởi động và kết nối thành công, ứng dụng tạo bảng users nếu bảng chưa tồn tại.

### Tầng dữ liệu

MySQL lưu bảng users với các trường id, name, email và role. Email được khai báo duy nhất; role gồm Admin và User. Trên EKS, MySQL chạy bằng StatefulSet, sử dụng PersistentVolumeClaim để lưu dữ liệu qua vòng đời của Pod.

## Luồng truy cập

1. Người dùng gửi yêu cầu đến NGINX Ingress Controller.
2. Ingress chuyển yêu cầu trang web đến frontend service.
3. Các yêu cầu API theo đường dẫn /api được chuyển đến backend service.
4. Backend đọc hoặc ghi dữ liệu người dùng trong MySQL.
5. MySQL lưu dữ liệu trên volume được cấp bởi Kubernetes storage.

Frontend và backend được triển khai trong các namespace riêng. Database cũng có namespace riêng để phân tách tài nguyên và cấu hình.

## Luồng phát hành

1. Nhà phát triển push thay đổi ứng dụng lên nhánh qa.
2. GitHub Actions chạy các bước bảo mật và các bước lint, test nếu package có khai báo scripts tương ứng.
3. Pipeline build image frontend và backend, sau đó đẩy image lên ECR.
4. Pipeline dùng YQ cập nhật tag image trong Helm values của repository IaC.
5. Pipeline commit thay đổi lên nhánh master của repository IaC.
6. Argo CD phát hiện thay đổi ở Git và đồng bộ cấu hình ứng dụng lên EKS.

## Thành phần kỹ thuật

1. AWS: Amazon EKS, Amazon ECR, VPC, IAM, NAT Gateway và Security Groups.
2. Infrastructure as Code: Terraform.
3. Container và triển khai: Docker, Kubernetes, Helm và NGINX Ingress Controller.
4. GitOps: Argo CD.
5. CI/CD: GitHub Actions và YQ.
6. Kiểm tra bảo mật: GitLeaks, Checkov và Trivy.
7. Monitoring có thể cài bổ sung: Prometheus, Grafana và Alertmanager.

## Phạm vi và trạng thái hiện tại

Frontend hiện hỗ trợ xem danh sách và thêm người dùng. Backend có thêm các API cập nhật và xóa, nhưng các nút Edit và Delete trên giao diện chưa được nối với các API này.

Workflow gọi lint và test bằng cơ chế chỉ chạy khi scripts tồn tại. Các package hiện tại chưa khai báo scripts lint hoặc test, vì vậy không nên mô tả pipeline hiện tại là đã chạy unit tests.

Terraform hiện chưa cài monitoring stack tự động. Prometheus, Grafana và Alertmanager cần được cài riêng trước khi sử dụng các script port-forward.
