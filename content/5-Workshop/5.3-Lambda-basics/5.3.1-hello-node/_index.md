---
title: "Lambda Hello World (Node.js)"
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

## Create the Hello World function (Node.js)

1) Open **Lambda console** -> **Create function** -> **Author from scratch**.  
2) Name: `hello-node`, Runtime: **Node.js 18.x**, Role: `AWSLambdaBasicExecutionRole`.  
3) Paste the sample handler:

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

4) **Save**.

## Test in the console

1) Click **Test** -> create a new event, body: `{"name":"Alice"}`.  
2) Run Test and check the response `Hello, Alice`.  
3) Open **Monitor -> Logs** to view the CloudWatch log stream.

## Basic configuration

- **Memory/Timeout:** increase/decrease as needed (e.g., 128–256 MB, 3–10s).  
- **Log retention:** open CloudWatch Log group `/aws/lambda/hello-node` -> Edit retention (7–14 days).  
- If `name` is missing, you can return 400 with a clear message (optional).