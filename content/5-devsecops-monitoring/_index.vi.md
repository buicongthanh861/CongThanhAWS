---
title: "DevSecOps & Monitoring"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

## Bảo mật trong quy trình phát triển

Workflow QA tích hợp các công cụ kiểm tra bảo mật để phát hiện sớm rủi ro trong mã nguồn, cấu hình hạ tầng và image ứng dụng.

### GitLeaks

GitLeaks quét lịch sử và nội dung repository để phát hiện secrets bị commit nhầm. Workflow lưu báo cáo scan thành artifact để nhóm phát triển có thể xem lại.

### Checkov

Checkov kiểm tra các cấu hình Infrastructure as Code và Kubernetes, đồng thời quét Dockerfiles. Báo cáo JSON được tạo để xem xét các cấu hình cần cải thiện.

Hiện lệnh Checkov được chạy với cơ chế cho phép workflow tiếp tục khi scanner trả về lỗi. Vì vậy kết quả scan có giá trị tham khảo nhưng chưa chặn việc build và phát hành.

### Trivy

Trivy quét filesystem của frontend và backend để tìm lỗ hổng phụ thuộc. Cấu hình tập trung vào mức HIGH và CRITICAL, bỏ qua một số lỗ hổng chưa có bản sửa và lưu kết quả thành artifact.

Đây là filesystem scan theo cấu hình workflow, không nên mô tả là đã quét image sau khi push lên ECR nếu không có bước image scan riêng.

## Monitoring và quan sát hệ thống

Repository IaC có các script port-forward cho Prometheus và Grafana. Khi monitoring stack đã cài và các service tồn tại trong namespace monitoring, người vận hành có thể truy cập dashboard cục bộ để quan sát CPU, memory, trạng thái Pod và số lần Pod restart.

Resource kube-prometheus-stack trong Terraform hiện đang bị comment. Do đó Terraform không cài Prometheus, Grafana hoặc Alertmanager tự động. Cần cài chart monitoring riêng trước khi chạy scripts/prometheus.sh hoặc scripts/grafana.sh.

Email receiver và routing cho Alertmanager chưa được xác nhận trong cấu hình được cung cấp. Nếu yêu cầu gửi cảnh báo qua email, cần khai báo receiver, thông tin SMTP trong Kubernetes Secret và các rule phù hợp.

## Quy trình xử lý cảnh báo đề xuất

1. Xác định chỉ số cần theo dõi như CPU, memory, Pod restart và trạng thái workload.
2. Tạo alert rule với ngưỡng và thời gian duy trì phù hợp để tránh cảnh báo nhiễu.
3. Kiểm tra Alertmanager nhận alert từ Prometheus.
4. Cấu hình receiver email và kiểm thử gửi nhận.
5. Ghi lại hướng dẫn xử lý cho từng loại cảnh báo.

Các bước trên là hướng hoàn thiện. Chỉ nên ghi là đã triển khai sau khi cài chart và kiểm tra cảnh báo thực tế thành công.
