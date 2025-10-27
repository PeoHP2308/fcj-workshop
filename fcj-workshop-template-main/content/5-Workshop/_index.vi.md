---
title: "Workshop"
date: "2025-10-06"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai Bảo mật và Kiểm soát Truy cập Chi tiết cho Truy cập S3 Hybrid

#### Tổng quan.

Ý Tưởng Lab Mở Rộng:
Bài lab này sẽ tập trung vào việc áp dụng các lớp bảo mật chồng chéo (Defense-in-Depth) để đảm bảo chỉ những nguồn được 
xác thực và cho phép mới có thể truy cập các tài nguyên S3 qua kết nối riêng tư (PrivateLink/VPC Endpoint).<br>
**Mục Tiêu Lab:** <br>
Sau khi hoàn thành bài lab này, bạn sẽ có thể:

Cấu hình **Interface VPC Endpoint** để mở rộng kết nối riêng tư **S3** đến môi trường **On-Premise (giả lập)**.

Thực thi **Chính sách Endpoint (Endpoint Policy)** để chỉ cho phép truy cập một bucket S3 cụ thể.

Áp dụng **Chính sách S3 Bucket** để giới hạn truy cập dựa trên Địa chỉ IP nguồn của trung tâm dữ liệu On-Premise.

Chứng minh rằng việc truy cập **S3** qua các endpoint được bảo mật và giới hạn nghiêm ngặt theo nguyên tắc **Least Privilege (Quyền tối thiểu)**.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Truy cập đến S3 từ VPC](5.3-S3-vpc/)
4. [Truy cập đến S3 từ TTDL On-premises](5.4-S3-onprem/)
5. [VPC Endpoint Policies (làm thêm)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)