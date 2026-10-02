---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Đóng gói ứng dụng với Helm

Repository IaC cung cấp ba Helm charts độc lập trong `k8s/`:

- **Frontend**: Deployment, ClusterIP Service, Ingress `/` và HPA từ 1–3 replica.
- **Backend**: Deployment, Service port `5000`, Ingress `/api`, HPA từ 1–3 replica, ConfigMap và Secret kết nối database.
- **Database**: MySQL `8.0` StatefulSet, service nội bộ, Secret và PVC `5Gi`.

Ingress frontend/backend được phục vụ qua NGINX Ingress Controller, cài bằng script vận hành với HTTP NodePort `30080`.

## Đồng bộ triển khai với Argo CD

**Argo CD** được sử dụng theo mô hình GitOps để theo dõi cấu hình khai báo trong Git và đồng bộ trạng thái mong muốn với Kubernetes cluster.

- Ba Argo CD Applications triển khai vào namespace riêng: `frontend`, `backend`, `database`.
- Theo dõi nhánh `master`, bật automated sync, prune resource đã xóa khỏi Git và self-heal khi cluster bị drift.
- Terraform cài Argo CD vào namespace `argocd`; server chạy insecure phía sau LoadBalancer.

Đảm bảo repository URL/branch trong `argocd/applications/` khớp với repository IaC thực tế.
