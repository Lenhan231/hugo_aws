---
title: "Add POST Method"
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

## Configure POST /hello

- Select the `/hello` resource → **Add method** → POST → **Lambda Function**.  
- Choose the same Lambda function used for GET.  
- Use **Lambda proxy integration** (recommended) so Lambda receives the full request; parse `event.body` (JSON) in the function.
![image](/images/5-Workshop/5.4-API-Gateway/5.4.2/create_get.png)
## Deploy & Test

- Redeploy the `dev` stage.  
- Test the endpoint:

```bash
curl -X POST -H "Content-Type: application/json" \\
  -d '{}' \\
  \"https://<id>.execute-api.<region>.amazonaws.com/dev/hello\"
```

- Ensure the JSON response returns the correct name; check CloudWatch logs if there are errors.

![image](/images/5-Workshop/5.4-API-Gateway/5.4.2/curl.png)
