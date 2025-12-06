---
title: "API Gateway Integration"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Goal

Publish Lambda through API Gateway (REST), create resources/methods, and test GET/POST with curl or Postman.

![image](/images/5-Workshop/Api_lambda.png)

## Steps

- **Create REST API**: new API, stage to a named environment (e.g., `dev`).  
- **Resource & methods**: create `/hello`, add `GET` and/or `POST`, choose Lambda integration, select your function.  
- **Deploy stage**: deploy to `dev` and note the invoke URL.  
- **Test**:  
  - GET: `curl "<invoke-url>/hello?name=alice"`  
  - POST: `curl -X POST -H "Content-Type: application/json" -d '{"name":"alice"}' "<invoke-url>/hello"`  
- **Handle input in Lambda**: read `event.queryStringParameters` for GET, parse `event.body` for POST; return JSON with proper status codes.