---
title: "Kiểm Thử API Gateway"
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

## Kiểm thử GET

1) Dùng curl:  
`curl "<invoke-url>/"`  
2) Kỳ vọng: HTTP 200, body `{"message":"Hello, tester"}`.  
3) Nếu sai tên hoặc lỗi 5xx, xem log CloudWatch.
```bash
PS C:\Users\Nhan\Documents\hugo_aws> curl "https://tyuyo8aqwd.execute-api.us-east-1.amazonaws.com/dev/hello"


StatusCode        : 200
StatusDescription : OK
Content           : {"message": "hello"}
RawContent        : HTTP/1.1 200 OK
                    x-amzn-RequestId:
                    07039249-41a2-4024-8939-5b8d676f46e7
                    x-amz-apigw-id: VLGW2G0loAMEm5Q=
                    X-Amzn-Trace-Id: Root=1-693450f8-a03e8d0a53d 
                    6c1ca901a1626;Parent=5c411597312990c1;Sample 
                    d=0;L...
Forms             : {}
Headers           : {[x-amzn-RequestId,
                    07039249-41a2-4024-8939-5b8d676f46e7],       
                    [x-amz-apigw-id, VLGW2G0loAMEm5Q=],
                    [X-Amzn-Trace-Id, Root=1-693450f8-a03e8d0a53 
                    d6c1ca901a1626;Parent=5c411597312990c1;Sampl 
                    ed=0;Lineage=1:b8101292:0], [Connection,     
                    Keep-Alive]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : System.__ComObject
RawContentLength  : 20

```
## Kiểm thử POST

1) Dùng curl/Postman:  
`curl -X POST -H "Content-Type: application/json" -d '{"name":"tester"}' "<invoke-url>/hello"`  
2) Kỳ vọng: JSON trả lời đúng tên, mã 200.  
3) Nếu Lambda không parse body, kiểm tra `event.body` và JSON.parse.

```bash
C:\Users\Nhan>curl -X POST -H "Content-Type: application/json" -d '{}' "https://tyuyo8aqwd.execute-api.us-east-1.amazonaws.com/dev/hello"
{"message": "hello"}
```
## Xử lý lỗi (tùy chọn)

- Thiết lập trả 400 khi thiếu `name` hoặc body không phải JSON.  
- Ghi log input để dễ debug: `console.log(event)` hoặc log các field cần thiết.



