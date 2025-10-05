---
title: "Tuần 1 – Mô-đun 1"
weight: 1
chapter: false
pre: "<b> 1.1. </b>"
date: 2025-09-09
status: "Hoàn thành"
---

# Mô-đun 1

**Ngày:** 09/09/2025  
**Trạng thái:** Hoàn thành  
**Tuần:** Tuần 1  

---

## Ghi chú bài giảng

### Điện toán đám mây là gì?

- Là việc phân phối các tài nguyên CNTT qua Internet với chính sách **trả tiền theo mức sử dụng**.

#### Lợi ích của điện toán đám mây

- Sử dụng bao nhiêu trả bấy nhiêu, giúp **tối ưu chi phí**.  
- Tăng tốc độ phát triển nhờ **tự động hóa và quản trị tập trung** từ nhà cung cấp.  
- Linh hoạt, có thể thêm bớt tài nguyên dễ dàng.  
- Dễ dàng mở rộng quy mô ứng dụng ra toàn cầu.

#### AWS — Điều gì tạo nên sự khác biệt?

- AWS là nhà cung cấp dịch vụ đám mây dẫn đầu **13 năm liên tiếp** (tính đến 2023).  
- AWS khác biệt nhờ **tầm nhìn dài hạn** và **văn hóa hướng khách hàng**.  
- Triết lý giá của AWS: khách hàng sẽ **ngày càng trả ít hơn** cho cùng một dịch vụ hoặc tài nguyên.  
- AWS luôn đặt **giá trị thật cho khách hàng** làm trung tâm trong các Nguyên tắc Lãnh đạo (Leadership Principles).  

![Tổng quan AWS](/images/1-Worklog/Week1/image.png)

---

### Bắt đầu hành trình “lên mây”

- Có nhiều khóa học về AWS, hoàn toàn có thể **tự học để trở thành chuyên gia**.  
- Đăng ký tài khoản AWS và trải nghiệm qua **Free Tier**.  
- Một số nền tảng học uy tín:
  - [Udemy](https://udemy.com/)
  - [Cloud Guru](https://cloudguru.com/)
- Lộ trình học AWS: [AWS Learning Paths](https://aws.amazon.com/vi/training/learning-paths)

---

## Hạ tầng AWS

### Trung tâm dữ liệu (Data Center)

- Mỗi trung tâm dữ liệu có thể chứa **hàng chục ngàn máy chủ**.  
- AWS sử dụng phần cứng **tối ưu hóa riêng** cho hạ tầng của mình.

### Availability Zone (AZ)

- Mỗi AZ bao gồm một hoặc nhiều trung tâm dữ liệu.  
- AZ được thiết kế để **cô lập lỗi (fault isolation)** — tránh việc hai AZ bị sự cố cùng lúc.  
- Các AZ kết nối với nhau qua **đường truyền tốc độ cao riêng biệt**.  
- AWS khuyến nghị nên triển khai ứng dụng trên ít nhất **2 AZ**.

### Region

- Mỗi **Region** gồm ít nhất **3 AZ** — hiện AWS có hơn **25 Region** trên toàn cầu.  
- Các Region được kết nối bằng **mạng backbone toàn cầu** của AWS.  
- Mặc định, dữ liệu và dịch vụ được tách biệt giữa các Region (trừ một số dịch vụ mang tính toàn cầu).

### Edge Locations

- Là hệ thống trung tâm dữ liệu biên giúp **giảm độ trễ truy cập**.  
- Một số dịch vụ hoạt động tại Edge:
  - CloudFront (CDN)  
  - Web Application Firewall (WAF)  
  - Route 53 (DNS Service)

---

## Công cụ quản lý dịch vụ AWS

### AWS Management Console — Đăng nhập Root

- Có thể đăng nhập bằng **tài khoản gốc (Root)** hoặc **IAM User** (tài khoản con có phân quyền truy cập tài nguyên).

### AWS Management Console — Đăng nhập IAM User

- Khi đăng nhập IAM User cần cung cấp thêm **Account ID** (chuỗi 12 chữ số) để định danh tài khoản AWS.

### AWS Management Console — Tìm kiếm dịch vụ

- Sau khi đăng nhập, có thể tìm kiếm các dịch vụ AWS.  
- Mỗi dịch vụ có **trang quản lý riêng** để sử dụng các tính năng của nó.

### AWS Management Console — Trung tâm hỗ trợ (Support Center)

- Ở góc phải giao diện, chọn **Support → Support Center** để tạo yêu cầu hỗ trợ (Support Case).

![Hỗ trợ AWS](/images/1-Worklog/Week1/image%20copy%204.png)

---

### AWS Command Line Interface (CLI)

- AWS CLI là **công cụ mã nguồn mở** cho phép quản lý dịch vụ AWS qua **dòng lệnh**.  
- Cung cấp chức năng tương đương với AWS Console trên trình duyệt, nhưng thao tác nhanh hơn.

![AWS CLI](/images/1-Worklog/Week1/image%20copy%207.png)

---

### AWS Software Development Kit (SDK)

- AWS SDK giúp **lập trình viên dễ dàng tương tác với AWS Services** thông qua thư viện chuẩn.  
- SDK quản lý các tác vụ như: **xác thực, retry, tuần tự hóa dữ liệu (serialization/deserialization)**,...

![AWS SDK](/images/1-Worklog/Week1/image%20copy%205.png)

---

## Tối ưu chi phí trên AWS

- Chọn cấu hình và vùng lưu trữ phù hợp.  
- Sử dụng các hình thức thanh toán ưu đãi (Reserved Instances, Savings Plans, Spot Instances).  
- Xóa tài nguyên không dùng, bật/tắt tự động với workload không chạy 24/7.  
- Tận dụng các dịch vụ **serverless**.  
- Thiết kế kiến trúc phù hợp để tối ưu hiệu năng và chi phí.  
- Cài đặt và sử dụng **AWS Budgets**.  
- Quản lý chi phí theo phòng ban hoặc ứng dụng bằng **cost allocation tags**.  
- Theo dõi và tối ưu chi phí liên tục.

**Công cụ ước tính chi phí:** [AWS Calculator](https://calculator.aws/#/)  
Cho phép tạo và chia sẻ bản ước tính, đồng thời xem sự khác biệt chi phí giữa các Region.

![Công cụ tính chi phí AWS](/images/1-Worklog/Week1/image%20copy%206.png)

---

## Làm việc với AWS Support

- AWS có 4 gói hỗ trợ chính:
  1. **Basic** (Khám phá)
  2. **Developer** (Phát triển / Kiểm thử)
  3. **Business** (Vận hành sản xuất)
  4. **Enterprise** (Doanh nghiệp lớn)
- Có thể nâng cấp tạm thời để xử lý sự cố quan trọng nhanh hơn.

---

## Thực hành

### Tạo tài khoản AWS

1. Tạo tài khoản AWS mới.  
2. Kích hoạt **MFA** cho tài khoản Root.  
3. Tạo nhóm **Admin** và gán người dùng.  
4. Xác thực thông tin tài khoản.  

### Bắt đầu với AWS Budgets

1. Tạo **Cost Budget**.  
2. Tạo **Usage Budget**.  
3. Tạo **Reservation Budget**.  
4. Tạo **Savings Plans Budget**.

### Gửi yêu cầu hỗ trợ qua AWS Support

1. Tìm hiểu các gói hỗ trợ AWS.  
2. Truy cập **Support Center**.  
3. Quản lý các **support case**.

---

## Nghiên cứu bổ sung

### AWS Well-Architected Framework

- Bộ nguyên tắc và thực hành tốt nhất để thiết kế hệ thống trên nền tảng đám mây.  
- Framework cung cấp câu hỏi đánh giá mức độ phù hợp của kiến trúc với best practices.  
- Khi dùng **Well-Architected Tool**, AWS gợi ý trực tiếp các hướng cải thiện kiến trúc trong Console.

---
