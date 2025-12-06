---
title: "Lambda cơ bản"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## Mục tiêu

Tạo và kiểm thử Lambda Hello World (Node.js/Python), truyền tham số đầu vào, và tinh chỉnh cấu hình (memory, timeout, log).

![image](/images/5-Workshop/5.3-Lambda-basics/Lambda_hello_world.png)

## Các bước chính

1) **Tạo hàm Node.js**  
   - Runtime: Node.js 18.x (ví dụ).  
![image](/images/5-Workshop/5.3-Lambda-basics/Lambda_nodejs_test.png)
   - Handler mẫu trả JSON `{ message: "hello" }`.  
![image](/images/5-Workshop/5.3-Lambda-basics/Lambda_nodejs_handler.png)
   - Test event với tham số `name`, log kết quả ở CloudWatch.
![image](/images/5-Workshop/5.3-Lambda-basics/Lambda_nodejs_cloudwatch.png)

2) **Tạo hàm Python**  
   - Runtime: Python 3.12 (ví dụ).
   - Handler đọc `event["name"]` và trả lời tùy biến.
![image](/images/5-Workshop/5.3-Lambda-basics/Alice_test.png)

3) **Tham số hóa & lỗi thường gặp**  
   - Đọc `event` (query/body sẽ được API Gateway gắn thêm ở bước sau).  
   - Bắt lỗi key thiếu, trả mã trạng thái phù hợp.
![image](/images/5-Workshop/5.3-Lambda-basics/404_not_name.png)

4) **Tối ưu nhẹ**  
   - Chỉnh `Memory` và `Timeout` để cân bằng chi phí/hiệu năng.  
   - Đặt CloudWatch retention.
![image](/images/5-Workshop/5.3-Lambda-basics/cloudwatch_retention.png)