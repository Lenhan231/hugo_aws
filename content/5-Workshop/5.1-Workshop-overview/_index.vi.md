---
title : "Giới thiệu"

weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Mục tiêu

Giới thiệu workshop serverless: tạo Lambda (Node.js/Python), nhận tham số đầu vào, tích hợp API Gateway, và triển khai API gợi ý in-app purchase. Nhấn mạnh cách giảm thời gian triển khai bằng Lambda, không cần quản lý server.

## Kiến trúc tổng quát

- Lambda nhận event, xử lý logic, trả JSON.  
- API Gateway làm lớp REST front door, map method → Lambda.  
- Tùy chọn: custom domain/stage cho nhiều môi trường.

## Kết quả mong đợi

- 2 hàm Hello World (Node.js/Python) có tham số.  
- 1 endpoint REST (GET/POST) qua API Gateway.  
- 1 hàm logic kinh doanh (gợi ý mua hàng) trả JSON.

## Thời lượng và cấu trúc

- Phần 1: Lambda cơ bản, test trong console.  
- Phần 2: API Gateway, mapping input, kiểm thử.  
- Phần 3: Bài tập in-app purchase, dọn dẹp tài nguyên.

![image](/images/5-Workshop/Api_lambda.png)
