---
title: "Lambda Hello World (Node.js)"
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

## Tạo hàm

1) Vào Lambda console → **Create function** → Author from scratch.  
2) Name: `hello-node`, Runtime: Node.js 18.x, Role: `AWSLambdaBasicExecutionRole`.  
3) Handler mẫu:

```javascript
exports.handler = async (event) => {
  const name =
    (event?.queryStringParameters || {}).name ||
    event?.name ||
    "world";
  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ message: `Hello, ${name}` })
  };
};
```

4) **Save** và tạo test event (ví dụ `{"name":"Alice"}`), xem log CloudWatch.
