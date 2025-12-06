---
title: "CORS & custom domain"
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

## Bật CORS

- Chọn method → **Enable CORS** → thêm header `Content-Type` và phương thức cần thiết.  
- Deploy lại stage sau khi bật.

## Custom domain (tùy chọn)

- API Gateway → **Custom domain names** → tạo domain và map stage `dev` → `/`.  
- Cập nhật DNS CNAME trỏ về domain API Gateway.

## Kiểm thử lại

- Gọi GET/POST qua domain mới hoặc với header CORS phù hợp.  
- Nếu browser báo CORS, kiểm tra lại cấu hình CORS và Deploy.
