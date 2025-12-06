---
title: "Test API Gateway"
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

## Test GET

1) Use curl:  
`curl "<invoke-url>/"`  
2) Expected: HTTP 200, body `{"message":"Hello, tester"}`.  
3) If the name is wrong or there's a 5xx error, check CloudWatch logs.
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
## Test POST

1) Use curl/Postman:  
`curl -X POST -H "Content-Type: application/json" -d '{"name":"tester"}' "<invoke-url>/hello"`  
2) Expected: JSON response with the correct name, status code 200.  
3) If Lambda doesn't parse the body, check `event.body` and JSON.parse.

```bash
C:\Users\Nhan>curl -X POST -H "Content-Type: application/json" -d '{}' "https://tyuyo8aqwd.execute-api.us-east-1.amazonaws.com/dev/hello"
{"message": "hello"}
```
## Error Handling (Optional)

- Return 400 when `name` is missing or body is not valid JSON.  
- Log input for easier debugging: `console.log(event)` or log required fields.



