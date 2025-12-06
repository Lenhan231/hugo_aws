---
title: "Sample App Logic"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Exercise: In-App Purchase Suggestions

Build a Lambda function that receives a list of purchased items and returns suggestions for items not yet purchased (simple set subtraction).

### Input Requirements

```json
{
  "allItems": ["starter_pack","booster_pack","data_pack","golden_apples","skins"],
  "owned": ["data_pack","starter_pack"]
}
```

### Processing

- Use set operations to get the remaining items: `suggestions = allItems - owned`.  
- Return JSON `{ "suggestions": [...] }` with status code 200.  
- Log input/output for easier debugging.

### Testing

- In the console: create a test event with various `owned` values.  
- Via API Gateway: `POST /suggest` with JSON body as shown above. 
![image](/images/5-Workshop/5.5-Sample-app/curl_post.png)
- Test error cases: missing `allItems` or `owned` → return 400 with a clear message.
![image](/images/5-Workshop/5.5-Sample-app/Missing.png)


