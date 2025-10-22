---
title: "Day 31 - Introduction to Serverless"
weight: 1
chapter: false
pre: "<b> 1.7.1. </b>"
---

**Date:** 2025-10-20 (Thứ Hai)  
**Status:** "Planned"  

---

# **Lecture Notes**

## Serverless Computing

### What is Serverless?

- **Serverless** doesn't mean "no servers" – it means you don't manage servers.
- AWS handles infrastructure provisioning, scaling, and maintenance.
- You focus on code, not infrastructure.

**Benefits:**

- No server management
- Automatic scaling
- Pay only for what you use
- Built-in high availability
- Faster time to market

---

## AWS Lambda

- **AWS Lambda** lets you run code without provisioning or managing servers.
- Supports multiple languages: Python, Node.js, Java, Go, .NET, Ruby, custom runtimes.

**Key Features:**

- Event-driven execution
- Automatic scaling (0 to thousands of concurrent executions)
- Subsecond billing (charged per 1ms)
- Integrated with 200+ AWS services
- Built-in fault tolerance

**Lambda Use Cases:**

- Real-time file processing
- Data transformation
- API backends
- Scheduled tasks (cron jobs)
- Stream processing
- IoT backends

**Lambda Pricing:**

- **Requests:** $0.20 per 1M requests
- **Duration:** Based on memory allocated and execution time
- **Free Tier:** 1M requests and 400,000 GB-seconds per month

---

## Lambda Function Structure

**Basic Python Example:**

```python
import json

def lambda_handler(event, context):
    # Event: input data
    # Context: runtime information
    
    name = event.get('name', 'World')
    
    return {
        'statusCode': 200,
        'body': json.dumps(f'Hello, {name}!')
    }
```

**Lambda Configuration:**

- **Memory:** 128 MB to 10,240 MB (CPU scales with memory)
- **Timeout:** Up to 15 minutes
- **Environment Variables:** Store configuration
- **Layers:** Share code and dependencies
- **VPC:** Access private resources

---

## Lambda Triggers

Lambda can be triggered by many AWS services:

- **API Gateway:** HTTP requests
- **S3:** Object uploads/deletes
- **DynamoDB:** Stream changes
- **SNS/SQS:** Messages
- **CloudWatch Events/EventBridge:** Scheduled or event-driven
- **Kinesis:** Stream processing
- **ALB:** Application Load Balancer requests

---

## Best Practices

- **Keep functions small and focused:** Single responsibility
- **Use environment variables:** For configuration
- **Implement proper error handling:** Try-catch blocks
- **Optimize cold starts:** Minimize dependencies, use provisioned concurrency
- **Monitor with CloudWatch:** Logs and metrics
- **Use Lambda Layers:** Share common code
- **Implement idempotency:** Handle duplicate invocations
- **Set appropriate timeouts:** Don't use max timeout unnecessarily

---

# **Hands-On Labs**

*Labs sẽ được cập nhật*
