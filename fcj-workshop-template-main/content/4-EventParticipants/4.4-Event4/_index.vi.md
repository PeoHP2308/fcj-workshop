---
title: "Event 4"
date: "2025-10-06"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# BÀI THU HOẠCH SỰ KIỆN: DEVOPS ON AWS

&emsp;**Ngày:** Thứ Hai, 17 tháng 11, 2025

&emsp;**Thời gian:** 8:30 AM – 5:00 PM

&emsp;**Địa điểm:** Văn phòng AWS

&emsp;**Mục tiêu sự kiện:** Cung cấp kiến thức toàn diện về Văn hóa DevOps, các nguyên tắc, chỉ số hiệu suất, và bộ công cụ dịch vụ DevOps của AWS để xây dựng quy trình CI/CD, IaC, và Observability.

---

## PHIÊN SÁNG (8:30 AM – 12:00 PM): CI/CD & INFRASTRUCTURE AS CODE

### 1. Chào mừng & Tư duy DevOps (8:30 – 9:00 AM)
* Sự kiện bắt đầu với phần **tổng kết nhanh** từ phiên AI/ML trước đó, đặt bối cảnh cho việc đưa các mô hình/ứng dụng này vào sản xuất (Production).
* Trọng tâm là **văn hóa và nguyên tắc DevOps**, nhấn mạnh sự hợp tác giữa Phát triển (Dev) và Vận hành (Ops).
* Giới thiệu các **chỉ số hiệu suất quan trọng** như **DORA** (Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate) và **MTTR** (Mean Time To Recovery).

### 2. Dịch vụ AWS DevOps – Xây dựng Pipeline CI/CD (9:00 – 10:30 AM)
Phần này đi sâu vào bộ công cụ AWS CodeFamily để tự động hóa quy trình **Tích hợp Liên tục (CI)** và **Triển khai Liên tục (CD)**:
* **Source Control:** Sử dụng **AWS CodeCommit** và thảo luận về các chiến lược quản lý mã nguồn như **GitFlow** và **Trunk-based**.
* **Build & Test:** Cấu hình **CodeBuild** để tự động biên dịch và chạy kiểm thử (testing pipelines).
* **Deployment (Triển khai):** Phân tích các chiến lược triển khai tiên tiến với **CodeDeploy**, bao gồm **Blue/Green, Canary, và Rolling updates**.
* **Orchestration (Điều phối):** Sử dụng **CodePipeline** để tự động hóa toàn bộ quy trình từ commit đến production.
* **Demo:** Trình diễn trực quan quy trình CI/CD hoàn chỉnh trên AWS.

### 3. Infrastructure as Code (IaC) (10:45 AM – 12:00 PM)
IaC là nền tảng của DevOps hiện đại, đảm bảo tính nhất quán và khả năng tái tạo của môi trường:
* **AWS CloudFormation:** Tìm hiểu về **Templates (Mẫu), Stacks (Ngăn xếp),** và cách sử dụng **Drift Detection** để phát hiện sự sai lệch giữa trạng thái thực tế và mã nguồn.
* **AWS CDK (Cloud Development Kit):** Giới thiệu về CDK như một phương pháp tiếp cận hiện đại hơn, sử dụng ngôn ngữ lập trình quen thuộc (TypeScript, Python,...) để định nghĩa hạ tầng thông qua **Constructs** và các **Mẫu tái sử dụng (reusable patterns)**.
* **Demo & Thảo luận:** Trình diễn triển khai hạ tầng bằng cả CloudFormation và CDK, cùng thảo luận về tiêu chí lựa chọn công cụ IaC phù hợp.

---

## PHIÊN CHIỀU (1:00 PM – 5:00 PM): CONTAINER, OBSERVABILITY & BEST PRACTICES

### 4. Dịch vụ Container trên AWS (1:00 – 2:30 PM)
* **Docker Fundamentals:** Ôn lại kiến thức cơ bản về **Microservices** và **Containerization**.
* **Amazon ECR (Elastic Container Registry):** Dịch vụ lưu trữ Image container, bao gồm tính năng **quét hình ảnh** và **chính sách vòng đời (lifecycle policies)**.
* **Amazon ECS & EKS:** So sánh hai dịch vụ điều phối container chính: **ECS** (đơn giản, tích hợp AWS) và **EKS** (dựa trên Kubernetes). Phân tích các chiến lược triển khai và mở rộng quy mô.
* **AWS App Runner:** Giải pháp đơn giản hóa việc triển khai container, tập trung vào code thay vì hạ tầng.
* **Demo & Case Study:** Minh họa việc triển khai kiến trúc microservices và so sánh sự khác biệt giữa các dịch vụ.

### 5. Giám sát & Khả năng Quan sát (Monitoring & Observability) (2:45 – 4:00 PM)
* **CloudWatch:** Công cụ cốt lõi cho việc thu thập **metrics, logs, alarms, và dashboards** trên toàn bộ hệ thống AWS.
* **AWS X-Ray:** Cung cấp **Distributed Tracing (truy vết phân tán)** để theo dõi luồng request qua các microservices, giúp xác định nút cổ chai và các vấn đề hiệu suất.
* **Demo:** Thiết lập hệ thống observability toàn diện (Full-stack observability).
* **Best Practices:** Các thực hành tốt nhất về **Alerting (cảnh báo), dashboards,** và quy trình **on-call** (trực sự cố).

### 6. Thực tiễn Tốt nhất & Case Studies (4:00 – 4:45 PM)
* **Deployment Strategies Nâng cao:** Thảo luận về **Feature flags** (cờ tính năng) và **A/B testing** trong quy trình triển khai.
* **Tự động hóa Kiểm thử (Automated testing)** và tích hợp sâu với CI/CD.
* **Incident Management:** Quản lý sự cố và tầm quan trọng của các báo cáo **Postmortems** (phân tích sau sự cố) để học hỏi và cải tiến.
* **Case Studies:** Bài học kinh nghiệm từ các công ty khởi nghiệp và doanh nghiệp lớn đã thực hiện chuyển đổi DevOps thành công.

### 7. Hỏi đáp & Tổng kết (4:45 – 5:00 PM)
* Giải đáp các thắc mắc chuyên sâu.
* Cung cấp thông tin về **lộ trình nghề nghiệp DevOps** và các chứng chỉ AWS liên quan.

---

## ĐÁNH GIÁ CHUNG VÀ ỨNG DỤNG

**Đánh giá:**
Sự kiện đã bao quát toàn bộ vòng đời DevOps, từ văn hóa, quy trình CI/CD, quản lý hạ tầng bằng mã, đến vận hành hệ thống container và observability. Kiến thức được truyền đạt rõ ràng, kết hợp giữa lý thuyết (DevOps Principles, DORA metrics) và công cụ thực tế (CodePipeline, CDK, X-Ray).

**Kiến thức Chính Thu được:**
* Nắm vững các công cụ AWS CodeFamily để thiết lập pipeline CI/CD tự động.
* Hiểu rõ ưu nhược điểm của CloudFormation và CDK trong IaC.
* Phân biệt và ứng dụng các dịch vụ container (ECS, EKS).
* Thiết lập hệ thống giám sát chủ động bằng CloudWatch và X-Ray.

**Hành động Đề xuất (Next Steps):**
1.  **Thực hành CI/CD:** Sử dụng **CodeCommit, CodeBuild, và CodePipeline** để tự động hóa việc triển khai cho dự án hiện tại (nếu có) hoặc một ứng dụng mẫu.
2.  **Khám phá CDK:** Bắt đầu sử dụng **AWS CDK** để định nghĩa hạ tầng project thay vì sử dụng Console, tập trung vào việc tạo các **Constructs** đơn giản.
3.  **Triển khai Observability:** Tích hợp **CloudWatch Logs** và thử nghiệm **AWS X-Ray** để theo dõi các chức năng (API calls) trong project nhằm áp dụng nguyên tắc giám sát ngay từ đầu.

## Một số hình ảnh khi tham gia sự kiện.

![](/images/4-Events/Event4.1.jpg)
![](/images/4-Events/Event4.2.jpg)
![](/images/4-Events/Event4.3.jpg)
![](/images/4-Events/Event4.4.jpg)

