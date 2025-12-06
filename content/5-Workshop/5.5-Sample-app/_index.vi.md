---
title: "Ứng dụng mẫu: Gợi ý in-app purchase"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Bài tập: Gợi ý in-app purchase

Xây dựng hàm Lambda nhận danh sách item đã mua và trả về gợi ý các item chưa mua (phép trừ tập hợp đơn giản).

### Yêu cầu đầu vào

```json
{
  "allItems": ["starter_pack","booster_pack","data_pack","golden_apples","skins"],
  "owned": ["data_pack","starter_pack"]
}
```

### Xử lý

- Dùng tập hợp để lấy phần còn lại: `suggestions = allItems - owned`.  
- Trả JSON `{ "suggestions": [...] }` và mã 200.  
- Log input/output để dễ debug.

### Kiểm thử

- Trong console: tạo test event với nhiều giá trị `owned`.  
- Qua API Gateway: `POST /suggest` body JSON như trên. 
![image](/images/5-Workshop/5.5-Sample-app/curl_post.png)
- Thử lỗi: thiếu `allItems` hoặc `owned` → trả 400 với thông báo rõ ràng.
![image](/images/5-Workshop/5.5-Sample-app/Missing.png)


