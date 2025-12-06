---
title: "API Gateway"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Mục tiêu

Xuất bản Lambda qua API Gateway (REST), tạo resource/method, và kiểm thử GET/POST bằng curl hoặc Postman.

![image](/images/5-Workshop/Api_lambda.png)

## Các bước

- Tạo REST API, resource `/hello`, bật CORS nếu cần.  
- Thêm method GET/POST, map tới Lambda (proxy integration).  
- Deploy stage `dev`, ghi lại Invoke URL.  
- Kiểm thử GET/POST, xử lý lỗi (400) khi thiếu dữ liệu.  
- Tùy chọn bật CORS hoặc custom domain.
