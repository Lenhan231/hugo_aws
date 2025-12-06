---
title: "Lambda Hello World (Python)"
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

## Tạo hàm

1) Vào Lambda console → **Create function** → Author from scratch.  
2) Name: `hello-python`, Runtime: Python 3.12.  
3) Handler mẫu:

```python
import json

def lambda_handler(event, context):
    name = (
        (event.get("queryStringParameters") or {}).get("name")
        or event.get("name")
        or "world"
    )
    return {
        "statusCode": 200,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps({"message": f"Hello, {name}"})
    }
```

4) **Save**, tạo test event (ví dụ `{"name":"Bob"}`), xem response và log CloudWatch.  
5) Có thể bắt lỗi body không hợp lệ và trả 400 nếu cần.

