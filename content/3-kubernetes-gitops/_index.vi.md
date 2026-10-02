---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Đóng gói ứng dụng với Helm

Ứng dụng được chia thành các **Helm Charts độc lập** cho:

- **Frontend**
- **Backend**
- **MySQL StatefulSet**

Việc tách chart giúp quản lý cấu hình, phiên bản và vòng đời triển khai từng thành phần rõ ràng hơn.

## Đồng bộ triển khai với Argo CD

**Argo CD** được sử dụng theo mô hình GitOps để theo dõi cấu hình khai báo trong Git và đồng bộ trạng thái mong muốn với Kubernetes cluster.

- Tự động đồng bộ cấu hình từ Git repository lên **Amazon EKS**.
- Phát hiện cấu hình lệch (drift detection) giữa Git và cluster.
- Tự khôi phục trạng thái mong muốn (self-healing) khi tài nguyên bị thay đổi ngoài quy trình.

Git trở thành nguồn tham chiếu cho cấu hình triển khai, hỗ trợ theo dõi lịch sử và review thay đổi.
