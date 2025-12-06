---
title: "Thêm phương thức POST"
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

## Cấu hình POST /hello

- Chọn resource `/hello` → **Add method** → POST → **Lambda Function**.  
- Chọn cùng Lambda đã dùng cho GET.  
- Dùng **Lambda proxy integration** để hàm nhận đầy đủ payload; trong handler parse `event.body` (JSON).
![image](/images/5-Workshop/5.4-API-Gateway/5.4.2/create_get.png)

## Triển khai & kiểm thử

- Deploy lại stage `dev`.  
- Gọi thử:

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"name":"api-user"}' \
  "<invoke-url>/hello"
```

- Đảm bảo response JSON trả lời đúng tên; kiểm tra log CloudWatch nếu lỗi parse.

![image](/images/5-Workshop/5.4-API-Gateway/5.4.2/curl.png)
