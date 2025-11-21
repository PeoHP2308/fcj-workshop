---
title: "Event 6"
date: "2025-10-06"
weight: 4
chapter: false
pre: " <b> 4.6. </b> "
---

# BÀI THU HOẠCH SỰ KIỆN: AWS CHO SAP SỬ DỤNG GENERATIVE AI
&emsp;**Thời gian:** 13:00 PM - 17:00 PM.

&emsp;**Speaker:** Nonthakorn Junthapol.

&emsp;**Chủ đề:** AWS for SAP Using Generative AI & SAP ABAP capabilities and Amazon Q Developer.

&emsp;**Mục tiêu sự kiện:** Khám phá cách Trí tuệ Nhân tạo Tạo sinh (GenAI) có thể tối ưu hóa và hiện đại hóa môi trường SAP, tập trung vào việc tăng cường năng suất phát triển ABAP thông qua Amazon Q Developer.

---

## BỐI CẢNH VÀ MỤC TIÊU CỦA GENAI TRONG SAP

### 1. Hiện đại hóa SAP trên AWS
* Sự kiện đặt nền tảng bằng cách khẳng định vai trò của AWS là nền tảng điện toán đám mây ưu tiên cho các hệ thống SAP S/4HANA (bao gồm cả mô hình **RISE with SAP**).
* Thảo luận về những thách thức truyền thống trong môi trường SAP: độ phức tạp của mã ABAP kế thừa (**Legacy ABAP**), nhu cầu hiện đại hóa (ví dụ: chuyển đổi sang **S/4HANA**), và tốc độ phát triển chậm.

### 2. Vai trò của Generative AI
* GenAI được giới thiệu như một công cụ chiến lược để vượt qua các thách thức trên, đặc biệt trong việc **tự động hóa code, refactoring, và tương tác dữ liệu**.
* Các dịch vụ như Amazon Bedrock (hoặc các mô hình tùy chỉnh trên SageMaker) có thể được sử dụng để phân tích dữ liệu SAP phi cấu trúc (ví dụ: tài liệu PO, Invoice) sau khi được trích xuất qua AWS.

---

## CHUYÊN SÂU: AMAZON Q DEVELOPER VÀ KHẢ NĂNG ABAP

### 3. Giới thiệu Amazon Q Developer
* **Amazon Q Developer** là trợ lý AI được thiết kế để tăng tốc phát triển phần mềm, được tùy chỉnh để hiểu bối cảnh hạ tầng và các API của AWS.
* Điểm nổi bật là khả năng **tương tác bằng ngôn ngữ tự nhiên** để giải quyết các vấn đề kỹ thuật và tạo mã.

### 4. Tăng cường năng suất với SAP ABAP
Phần trọng tâm của sự kiện đi sâu vào cách Amazon Q hỗ trợ trực tiếp các nhà phát triển ABAP:

| Khả năng của Amazon Q                | Ứng dụng trong ABAP                                                                                                                 | Giá trị mang lại                                    |
|:-------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------|
| **Code Generation (Tạo Mã)**         | Tạo nhanh các đoạn mã ABAP cho BAPI, Function Modules, hoặc các báo cáo ALV tiêu chuẩn.                                             | Tăng tốc độ coding ban đầu.                         |
| **Code Explanation (Giải thích Mã)** | Phân tích và giải thích các đoạn mã ABAP kế thừa phức tạp hoặc ít được sử dụng.                                                     | Giảm thời gian bảo trì và học lại mã.               |
| **Refactoring & Modernization**      | Hỗ trợ chuyển đổi mã ABAP cũ sang cú pháp và nguyên tắc lập trình hướng đối tượng (OOP) hiện đại hơn (ví dụ: chuẩn bị cho S/4HANA). | Giảm rủi ro và chi phí di chuyển/nâng cấp hệ thống. |
| **Troubleshooting (Gỡ lỗi)**         | Đề xuất giải pháp và hướng dẫn gỡ lỗi dựa trên thông báo lỗi (error messages) trong môi trường SAP.                                 | Giảm thiểu thời gian ngừng hoạt động (MTTR).        |

### 5. Cơ chế Tích hợp và Bảo mật
* Amazon Q hoạt động như một lớp trừu tượng, truy cập vào mã nguồn ABAP thông qua các kết nối được ủy quyền (có thể thông qua các công cụ AWS Connector hoặc tích hợp môi trường phát triển).
* **Bảo mật dữ liệu:** Nhấn mạnh rằng dữ liệu mã nguồn ABAP (Source Code) được xử lý an toàn và không được sử dụng để huấn luyện mô hình cơ bản của Amazon Q, đảm bảo quyền riêng tư và bảo mật tài sản trí tuệ của doanh nghiệp.

---

## ĐÁNH GIÁ CHUNG VÀ HÀNH ĐỘNG TIẾP THEO

**Đánh giá:**
Sự kiện đã chỉ ra một hướng đi rõ ràng cho tương lai của việc phát triển SAP ABAP, chuyển từ một quy trình thủ công và tốn thời gian sang một quy trình được hỗ trợ bởi AI. Amazon Q Developer đóng vai trò là công cụ đột phá, giúp các doanh nghiệp SAP tận dụng triệt để năng lực của AWS Cloud không chỉ ở tầng hạ tầng mà còn ở tầng ứng dụng và phát triển.

**Hành động Đề xuất (Next Steps):**
1.  **Thử nghiệm Amazon Q Developer:** Các nhóm phát triển SAP nên triển khai Amazon Q Developer trong môi trường phát triển sandbox để kiểm tra khả năng tạo, giải thích, và refactor các đoạn mã ABAP tiêu chuẩn.
2.  **Lập kế hoạch Refactoring:** Sử dụng Amazon Q để đánh giá mức độ phức tạp và rủi ro của mã ABAP kế thừa, nhằm lập kế hoạch chi tiết cho các dự án hiện đại hóa hoặc di chuyển lên S/4HANA (Greenfield/Brownfield).
3.  **Tích hợp GenAI vào Data Flow:** Nghiên cứu cách sử dụng các dịch vụ AWS GenAI (ngoài Q Developer) để xử lý các luồng dữ liệu nghiệp vụ của SAP (ví dụ: tự động tóm tắt các đơn hàng lớn, phân loại lỗi giao dịch).

## Một số hình ảnh khi tham gia sự kiện.

![](/images/4-Events/Event6.1.jpg)  
![](/images/4-Events/Event6.2.jpg)  

