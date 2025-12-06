---
title: "Lambda Hello World (Python)"
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

## Create the function

1) Lambda console -> **Create function** -> Author from scratch.  
2) Name: `hello-python`, Runtime: Python 3.12.  
3) Handler:

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

4) Save, create a test event (e.g., `{"name":"Bob"}`), and view CloudWatch logs.  
5) Optional: add a 400 response for missing/invalid input.

