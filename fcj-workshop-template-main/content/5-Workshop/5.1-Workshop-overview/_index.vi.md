---
title : "Giới thiệu"
date :  "2025-10-06" 
weight : 5
chapter : false
pre : " <b> 5.1. </b> "
---

### Giới thiệu về VPC Endpoint

- **VPC Endpoint** là một **thiết bị ảo** trong Amazon VPC, cho phép các tài nguyên điện toán (như EC2, Lambda, ECS,...) 
**giao tiếp an toàn** với các dịch vụ AWS mà **không cần đi qua Internet công cộng**.
- Các Endpoint này được thiết kế **mở rộng theo chiều ngang**, **dự phòng cao**, và **đảm bảo tính sẵn sàng**, 
giúp loại bỏ rủi ro mất kết nối do phụ thuộc vào Internet Gateway hoặc NAT Gateway.
- Tài nguyên điện toán chạy trong **VPC** có thể truy cập **Amazon S3** thông qua **Gateway Endpoint**, 
trong khi **Interface Endpoint (AWS PrivateLink)** có thể được sử dụng cho các tài nguyên **trong VPC hoặc 
tại Trung tâm dữ liệu (On-Premises)** để truy cập các dịch vụ AWS một cách riêng tư.

---

### Tổng quan về Workshop.

Trong workshop này, chúng ta sẽ:
- Tìm hiểu cách **tạo, cấu hình và kiểm tra** hai loại **VPC Endpoint** (Gateway và Interface).
- Thiết lập mô hình **Hybrid Access**, nơi workload trong VPC và hệ thống On-Premises đều có thể truy cập 
**Amazon S3** một cách **bảo mật, riêng tư và không qua Internet**.
- Áp dụng **các lớp chính sách bảo mật (Security Layers)** gồm:
    + **Endpoint Policy**: Kiểm soát quyền truy cập tại cấp độ Endpoint.
    + **Bucket Policy**: Giới hạn quyền truy cập dựa trên IP nguồn và phạm vi mạng cho phép.
- Thực hiện **kiểm thử truy cập (Positive & Negative Testing)** để xác minh hoạt động của từng lớp bảo mật và 
- đảm bảo chỉ các nguồn hợp lệ mới có thể truy cập dữ liệu trong S3.

### Các Bước Thực Hiện

####  Giai đoạn 1: Chuẩn bị Môi trường.

| Bước | Hành động | Vai trò |
|------|------------|---------|
| 1.1 | Tạo một **VPC** (ví dụ: `Hybrid-VPC`) và 2 **Subnet** (1 Public, 1 Private). | Môi trường Cloud |
| 1.2 | Tạo một **S3 Bucket** (ví dụ: `s3://hybrid-secure-data-prod`). | Nơi chứa dữ liệu |
| 1.3 | Tạo một **EC2 Instance** trong **Subnet Private** (giả lập Workload trong VPC). | Đối tượng kiểm tra 1 |
| 1.4 | Tạo một **EC2 Instance** trong **Subnet Public** (giả lập Hệ thống On-Premise) và gán một IP nguồn cụ thể (giả định đây là IP đi qua Direct Connect/VPN). | Đối tượng kiểm tra 2 |

---

####  Giai đoạn 2: Cấu hình Interface Endpoint (Cho Truy cập Hybrid).

| Bước | Hành động                                                                                                                               | Mục đích                                                      |
|------|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| 2.1  | Tạo **Interface VPC Endpoint** cho S3. Chọn `Service Name` là `com.amazonaws.region.s3`.                                                | Thiết lập kết nối riêng tư cho On-Premise.                    |
| 2.2  | Cấu hình **DNS Private** trong VPC để lưu lượng từ On-Premise phân giải tên S3 thành IP riêng của Endpoint.                             | Đảm bảo lưu lượng không đi ra Internet.                       |
| 2.3  | Áp dụng **Chính sách Endpoint (Policy)** sau cho Interface Endpoint, chỉ cho phép truy cập đến `arn:aws:s3:::hybrid-secure-data-prod*`. | Lớp Bảo mật 1: Hạn chế Bucket được truy cập qua Endpoint này. |

#### JSON Policy:
```json
{
  "Statement": [
    {
      "Sid": "AllowAccessToSpecificBucket",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::hybrid-secure-data-prod",
        "arn:aws:s3:::hybrid-secure-data-prod/*"
      ]
    },
    {
      "Sid": "DenyAllOtherBuckets",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::*"
    }
  ]
}
```

####  Giai đoạn 3: Cấu hình Gateway Endpoint (Cho Truy cập trong VPC).

| Bước  | Hành động                                                                                                                                  | Mục đích                                                      |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| 3.1   | Tạo **Gateway VPC Endpoint** cho S3.                                                                                                       | Đảm bảo truy cập S3 trong VPC mà không cần đi qua Internet.   |
| 3.2   | **Cập nhật Route Table**: Liên kết Endpoint với Route Table của Subnet Private.                                                            | Điều hướng lưu lượng S3 trong VPC đến Gateway Endpoint.       |
| 3.3   | Áp dụng **Chính sách Endpoint**: Áp dụng một chính sách đơn giản cho phép truy cập, nhưng chỉ dành cho các IAM Role/User cụ thể trong VPC. | **Lớp Bảo mật 2:** Cơ chế kiểm soát truy cập bổ sung.         |

---

####  Giai đoạn 4: Thực thi Chính sách S3 Bucket (Kiểm soát Nguồn).

| Bước  | Hành động                                                                                                                                                                     | Mục đích                                                                                        |
|-------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| 4.1   | Cấu hình **S3 Bucket Policy** cho `s3://hybrid-secure-data-prod`.                                                                                                             | **Lớp Bảo mật 3:** Lớp kiểm soát cuối cùng, đảm bảo chỉ những nguồn đáng tin cậy mới được phép. |
| 4.2   | Sử dụng điều kiện **aws:SourceIp** để chỉ cho phép truy cập từ **CIDR Block của On-Premise (IP của EC2 giả lập ở 1.4)** và **CIDR Block của Subnet Private (EC2 trong VPC)**. | Giới hạn truy cập vào S3 từ bên ngoài các nguồn Hybrid được phép.                               |

#### JSON Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RestrictAccessToHybridSources",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::hybrid-secure-data-prod",
        "arn:aws:s3:::hybrid-secure-data-prod/*"
      ],
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": [
            "IP_CỦA_ON_PREM_GIẢ_LẬP/32",
            "CIDR_CỦA_SUBNET_PRIVATE"
          ]
        }
      }
    }
  ]
}
```
#### Giai đoạn 5: Kiểm tra và Thử nghiệm Thất bại (Negative Testing)

Mục tiêu của giai đoạn này là **xác minh tính hiệu quả của các lớp bảo mật** được áp dụng trong cấu hình VPC Endpoint và S3 Bucket Policy, đảm bảo rằng chỉ các nguồn hợp lệ (được định danh qua IP và Endpoint Policy) mới có quyền truy cập.

| **Bước**  | **Nguồn truy cập**                | **Mục tiêu Kiểm tra**                                | **Kết quả Mong đợi**                                                  |
|-----------|-----------------------------------|------------------------------------------------------|-----------------------------------------------------------------------|
| **5.1**   | EC2 On-Premise *(IP được phép)*   | Truy cập `s3://hybrid-secure-data-prod`              | ✅ **THÀNH CÔNG:** Được phép vì IP nguồn và Bucket Policy khớp.        |
| **5.2**   | EC2 On-Premise *(IP được phép)*   | Truy cập một bucket S3 **khác**                      | ❌ **THẤT BẠI:** Bị từ chối bởi **Interface Endpoint Policy**.         |
| **5.3**   | EC2 trong VPC *(Subnet Private)*  | Truy cập `s3://hybrid-secure-data-prod`              | ✅ **THÀNH CÔNG:** Được phép vì IP nguồn và Bucket Policy khớp.        |
| **5.4**   | EC2 bất kỳ *(có Public IP)*       | Truy cập `s3://hybrid-secure-data-prod` qua Internet | ❌ **THẤT BẠI:** Bị từ chối bởi **S3 Bucket Policy** (`NotIpAddress`). |

---

### Phân tích kết quả kiểm thử.

- **Truy cập hợp lệ (5.1 & 5.3):**  
  Các nguồn trong danh sách IP cho phép hoặc từ Subnet Private được truy cập bình thường thông qua các Endpoint bảo mật.

- **Truy cập bị từ chối (5.2 & 5.4):**  
  Các truy cập nằm ngoài phạm vi IP hoặc bucket được phép đều bị từ chối ở lớp **Endpoint Policy** hoặc 
- **Bucket Policy**, đảm bảo nguyên tắc **Zero Trust Access**.

---

### Kết luận.

Việc thử nghiệm đã xác nhận rằng:
- Các **Gateway và Interface Endpoint** hoạt động đúng chức năng, định tuyến lưu lượng nội bộ mà không đi qua Internet.
- **Chính sách bảo mật nhiều lớp** (VPC Endpoint Policy + S3 Bucket Policy) hoạt động hiệu quả.
- **Mô hình Hybrid Access** được đảm bảo an toàn, kiểm soát được nguồn truy cập, và tuân thủ nguyên tắc bảo mật nội bộ của AWS.
