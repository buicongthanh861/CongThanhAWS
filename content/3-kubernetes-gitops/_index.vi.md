---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Đóng gói ứng dụng bằng Helm

Repository hạ tầng có ba Helm charts độc lập trong thư mục k8s. Mỗi chart quản lý cấu hình và tài nguyên của một thành phần ứng dụng.

### Frontend

Frontend được chạy bằng Kubernetes Deployment và được cung cấp trong cluster qua Service loại ClusterIP. Ingress sử dụng đường dẫn gốc / để tiếp nhận yêu cầu giao diện. Horizontal Pod Autoscaler được cấu hình trong khoảng một đến ba replica theo mức sử dụng CPU.

### Backend

Backend chạy bằng Deployment và Service nội bộ trên cổng 5000. Ingress nhận yêu cầu tại đường dẫn /api và cấu hình rewrite theo manifest. Chart có ConfigMap và Secret để truyền cấu hình kết nối cơ sở dữ liệu. HPA cho phép số replica từ một đến ba theo CPU.

### Database

MySQL 8 chạy bằng StatefulSet cùng service nội bộ. Thông tin kết nối được lưu trong Kubernetes Secret. PersistentVolumeClaim 5 GiB được sử dụng để lưu dữ liệu ngoài vòng đời của Pod.

## Ingress và truy cập ứng dụng

NGINX Ingress Controller được cài qua scripts/nginx.sh bằng Helm. Script cấu hình HTTP NodePort 30080. Ingress định tuyến đường dẫn / đến frontend và /api đến backend.

Frontend Dockerfile phục vụ nội dung tĩnh bằng Nginx. Cần kiểm tra cấu hình proxy và đường dẫn API khi triển khai vì Nginx trong image frontend không mặc nhiên chuyển tiếp /api sang backend nếu chưa được cấu hình.

## GitOps với Argo CD

Terraform cài Argo CD trong namespace argocd. Các Application manifests nằm trong argocd/applications và theo dõi repository IaC.

1. Argo CD theo dõi nhánh master và các thư mục chart frontend, backend, database.
2. Mỗi ứng dụng được cài vào namespace riêng có cùng tên với thành phần.
3. Automated sync áp dụng thay đổi từ Git vào cluster.
4. Prune xóa tài nguyên Kubernetes không còn được khai báo trong Git.
5. Self-heal đưa tài nguyên đã bị thay đổi thủ công trở lại trạng thái trong Git.

Trước khi triển khai, xác nhận URL repository, nhánh master và đường dẫn chart trong từng Application manifest. Nếu các giá trị này không khớp repository thực tế, Argo CD sẽ không thể đồng bộ đúng cấu hình.

## Script vận hành

1. scripts/connect.sh cập nhật kubeconfig cho EKS và kiểm tra node.
2. scripts/nginx.sh cài NGINX Ingress Controller bằng Helm.
3. scripts/argocd.sh mở port-forward Argo CD đến localhost cổng 8080 và hiển thị mật khẩu admin ban đầu.
4. scripts/grafana.sh mở port-forward Grafana đến localhost cổng 3000.
5. scripts/prometheus.sh mở port-forward Prometheus đến localhost cổng 9090.
6. scripts/start-all.sh cập nhật kubeconfig, cài NGINX và khởi chạy các port-forward ở chế độ nền.

Các script được viết bằng Bash và sử dụng tiện ích như /tmp và pkill. Chạy trong Linux, WSL hoặc Git Bash phù hợp; cần kiểm tra quyền thực thi trước khi chạy.

## Thứ tự triển khai workload

1. Hoàn tất Terraform apply và xác nhận các node EKS ở trạng thái Ready.
2. Chạy script kết nối cluster và cài NGINX Ingress Controller.
3. Áp dụng các manifest trong argocd/applications.
4. Kiểm tra trạng thái Argo CD Application và chờ sync hoàn tất.
5. Kiểm tra Deployment, Pod, Service, PVC và Ingress trong các namespace frontend, backend và database.

## Cấu hình cần kiểm tra

Backend được cấu hình kết nối đến service database trong namespace database, sử dụng database test_db và cổng MySQL mặc định. Xác nhận hostname, username, password và tên database trong Helm values và Secret khớp với cấu hình của MySQL.

Frontend và backend có resource requests, limits và HPA theo CPU ở mức 50 phần trăm. Đảm bảo cluster có metrics provider hoạt động để HPA đọc được chỉ số.

Database cần PVC 5 GiB với access mode ReadWriteOnce. Cấu hình hiện khai báo gp3 trong values nhưng template StatefulSet sử dụng gp2 cố định; cần sửa cho nhất quán trước khi phụ thuộc vào giá trị trong values.
