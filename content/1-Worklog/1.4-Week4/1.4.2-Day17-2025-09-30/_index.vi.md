---
title: "Ngày 17 - Tính năng nâng cao của S3"
weight: 2
chapter: false
pre: "<b> 1.4.2. </b>"
---

**Ngày:** 2025-09-30 (Thứ Ba)  
**Trạng thái:** "Hoàn thành"  

---

# **Ghi chú Bài học**

## Hosting website tĩnh trên Amazon S3

Hosting trực tiếp website tĩnh (HTML, CSS, JS, hình ảnh) từ S3.

### Khả năng chính

- **Thiết lập đơn giản:** Chỉ vài bước để bật chế độ static website cho bucket.  
- **Chi phí thấp:** Trả phí lưu trữ và băng thông tiêu chuẩn, không cần máy chủ web riêng.  
- **Scale linh hoạt:** Tự động xử lý spike traffic.  
- **Tích hợp CDN:** Dễ dàng kết hợp Amazon CloudFront để tăng hiệu năng toàn cầu.

**Cấu hình website tĩnh:**

```json
{
  "IndexDocument": {
    "Suffix": "index.html"
  },
  "ErrorDocument": {
    "Key": "error.html"
  }
}
```
