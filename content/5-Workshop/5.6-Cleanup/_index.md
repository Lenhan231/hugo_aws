---
title: "Clean up"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Cleanup Steps

- Delete the API Gateway stage/API created for the workshop.  
![image](/images/5-Workshop/5.6-Cleanup/api.png)
- Delete the Lambda functions (Node.js, Python, suggest).  
![image](/images/5-Workshop/5.6-Cleanup/lambda.png)
- Remove IAM roles/policies created specifically for Lambda (if no longer needed).
![image](/images/5-Workshop/5.6-Cleanup/iam_role.png)  
- Check CloudWatch Log Groups, set short retention or delete.
![image](/images/5-Workshop/5.6-Cleanup/cloudwatch.png)  
- Ensure no remaining resources that incur costs (temporary S3 buckets, temporary Secrets if any).


## Cost Notes

- Lambda/Logs/API Gateway have low costs but should still be deleted after the lab.  
- If you created additional resources (S3, KMS), confirm they have been deleted/retention set.
