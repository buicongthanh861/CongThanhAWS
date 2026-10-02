---
title: "Infrastructure & AWS"
weight: 2
chapter: false
pre: "<b>2. </b>"
---

## Mục tiêu hạ tầng

Repository 3-tier-user-platform-iac quản lý tài nguyên AWS và cấu hình Kubernetes add-ons bằng Terraform. Cách tiếp cận Infrastructure as Code giúp lưu cấu hình trong Git, xem trước thay đổi bằng plan và triển khai lại môi trường từ các tệp cấu hình.

## Mạng VPC

1. VPC sử dụng dải địa chỉ 10.0.0.0/16.
2. Môi trường triển khai tại region ap-southeast-1.
3. Subnet được bố trí tại hai Availability Zones ap-southeast-1a và ap-southeast-1b.
4. Public subnet phục vụ các thành phần cần điểm vào từ bên ngoài theo cấu hình mạng.
5. Private subnet dành cho node và workload không cần địa chỉ truy cập công khai trực tiếp.
6. Internet Gateway, NAT Gateway, Elastic IP, route tables và Security Groups hỗ trợ kết nối giữa các lớp mạng.

Cấu hình hiện tại sử dụng một NAT Gateway. Cần đánh giá chi phí và tính sẵn sàng theo từng Availability Zone trước khi dùng production.

## Amazon EKS

Cluster được khai báo với tên staging-demo-eks. Environment mặc định là staging. Phiên bản Kubernetes được cấu hình trong terraform/locals.tf.

Managed node group tên general sử dụng instance type t3.medium. Cấu hình có hai node mặc định và cho phép mở rộng tối đa năm node. Việc mở rộng thực tế phụ thuộc quota AWS, tải của ứng dụng và cấu hình autoscaling.

## IAM và quyền truy cập

Terraform khai báo IAM roles và policies cho EKS control plane, worker nodes, quyền đọc image từ ECR và các add-ons cần thiết. OIDC provider được cấu hình để Kubernetes service accounts có thể nhận IAM role thông qua IRSA, hạn chế việc cấp quyền AWS trực tiếp cho toàn bộ worker node.

Quyền IAM cần được rà soát theo nguyên tắc cấp quyền tối thiểu. Không sử dụng access key dài hạn trong ứng dụng nếu có thể thay thế bằng IAM role.

## Kubernetes add-ons

Terraform cài Argo CD vào namespace argocd. Argo CD server được cấu hình chạy insecure phía sau LoadBalancer; cần kiểm soát phạm vi truy cập và cấu hình TLS ở lớp ingress hoặc load balancer trước khi sử dụng công khai.

Các add-ons khác gồm AWS Load Balancer Controller để quản lý tài nguyên load balancer và EBS CSI driver để cấp phát volume cho PersistentVolumeClaim. StorageClass gp2 được đặt làm mặc định trong cấu hình Kubernetes.

## Cấu trúc repository hạ tầng

1. Thư mục terraform chứa AWS infrastructure và Kubernetes add-ons.
2. Thư mục argocd/applications chứa các Argo CD Application manifests.
3. Thư mục k8s/frontend chứa Helm chart frontend.
4. Thư mục k8s/backend chứa Helm chart backend.
5. Thư mục k8s/database chứa Helm chart MySQL.
6. Thư mục scripts chứa lệnh kết nối cluster, cài ingress và mở port-forward.

## Triển khai hạ tầng

1. Cài AWS CLI, Terraform, kubectl và Helm.
2. Cấu hình AWS credentials có quyền tạo các tài nguyên trong project.
3. Vào thư mục terraform và chạy terraform init để khởi tạo provider và module.
4. Chạy terraform validate để kiểm tra cú pháp cấu hình.
5. Chạy terraform plan và rà soát tài nguyên sẽ được tạo hoặc thay đổi.
6. Chạy terraform apply sau khi xác nhận kế hoạch.
7. Dùng scripts/connect.sh để cập nhật kubeconfig và kiểm tra node trong cluster.

## Kết quả và lưu ý

Thời gian provisioning được báo cáo là dưới 15 phút. Thời gian thực tế thay đổi theo trạng thái dịch vụ AWS, quota, vùng triển khai và các tài nguyên cần tạo.

Chart database có điểm cần đồng bộ: values.yaml khai báo StorageClass gp3 nhưng StatefulSet template hiện cố định gp2. Kiểm tra template và StorageClass trước khi triển khai để tránh giá trị khai báo không có hiệu lực.

Terraform state có thể chứa thông tin nhạy cảm. Không commit state, tfvars chứa bí mật hoặc credentials. Nên cấu hình remote backend có encryption và state locking trước khi làm việc theo nhóm.

Không chạy terraform destroy nếu chưa xác nhận phạm vi ảnh hưởng. Lệnh này có thể xóa cluster, node group, network và các tài nguyên AWS do Terraform quản lý.
