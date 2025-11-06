---
title: "Ngày 16 - Kiến thức cơ bản về Amazon S3"
weight: 1
chapter: false
pre: "<b> 1.4.1. </b>"
---

**Ngày:** 2025-09-29 (Thứ Hai)  
**Trạng thái:** "Hoàn thành"  

---

# **Ghi chú Bài học**

## Dịch vụ lưu trữ trên AWS

### Amazon Simple Storage Service (S3)

Amazon S3 là dịch vụ lưu trữ đối tượng, cho phép lưu và truy xuất dữ liệu với quy mô gần như không giới hạn, độ sẵn sàng cao, bảo mật mạnh và hiệu năng tốt.

### Tính năng cốt lõi của S3

- **Bucket và Object:** Dữ liệu được lưu dưới dạng object trong bucket. Mỗi object tối đa 5 TB.  
- **Availability & Durability:** Thiết kế đạt 99,99% availability và 99,999999999% (11 số 9) durability.  
- **Bảo mật:** Nhiều lớp bảo vệ như IAM, bucket policy, ACL, mã hóa.  
- **Khả năng mở rộng:** Tự động scale dung lượng và throughput mà không giảm hiệu năng.

**Cấu trúc object S3:**

- **Key:** Tên/đường dẫn object.  
- **Value:** Dữ liệu object.  
- **Version ID:** Dùng khi bật versioning.  
- **Metadata:** Metadata hệ thống và người dùng.  
- **Access Control:** Quyền truy cập.

### S3 Access Points

Access Point giúp đơn giản hóa việc quản lý truy cập cho dataset dùng chung.

- **Kiểm soát theo ứng dụng:** Mỗi access point có policy riêng.
