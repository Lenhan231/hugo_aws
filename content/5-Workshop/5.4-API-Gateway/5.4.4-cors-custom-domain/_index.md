---
title: "Optional: CORS & Custom Domain"
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

## Enable CORS

- Select the method → Enable CORS → add the `Content-Type` header and required methods.  
- Redeploy the stage after enabling CORS.

## Custom Domain (Optional)

- API Gateway → Custom domain names → create a domain and map the `dev` stage → `/`.  
- Update DNS CNAME to point to the API Gateway domain.

## Retest

- Call GET/POST again through the new domain or with appropriate CORS headers.  
- Check logs if the browser blocks the request due to CORS.

