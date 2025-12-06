---
title: "Chuẩn bị"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Tài khoản & quyền hạn

- AWS account với quyền tạo Lambda, CloudWatch Logs, API Gateway (IAM tối thiểu: `AWSLambdaBasicExecutionRole`).  
- Không cần quyền EC2/VPC nâng cao cho workshop này.

## Công cụ

- Trình duyệt để thao tác AWS Console.  
- curl/Invoke-RestMethod hoặc Postman để gọi API Gateway.  
- Tùy chọn: editor cục bộ để soạn code Node.js/Python trước khi dán vào console.

## Thiết lập nhanh

- Chọn region gần (ví dụ us-east-1 hoặc ap-southeast-1).  
- Tạo IAM role cho Lambda với trust policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["sts:AssumeRole"],
      "Principal": { "Service": ["lambda.amazonaws.com"] }
    }
  ]
}
```
- Gán policy `AWSLambdaBasicExecutionRole` cho role.  
- Đặt CloudWatch Logs retention (ví dụ 7–14 ngày) để kiểm soát chi phí: CloudWatch → Log groups → `/aws/lambda/<tên-hàm>` → Actions → Edit retention.

![image](/images/5-Workshop/5.2-Prerequisite/Iamrole_for_lambda.png)