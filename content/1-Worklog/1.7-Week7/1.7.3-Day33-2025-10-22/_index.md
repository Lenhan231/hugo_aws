---
title: "Day 33 - Monitoring & Logging"
weight: 3
chapter: false
pre: "<b> 1.7.3. </b>"
---

**Date:** 2025-10-22 (Thứ Tư)  
**Status:** "Planned"  

---

# **Lecture Notes**

## AWS Monitoring Services

### Amazon CloudWatch

- **Amazon CloudWatch** monitors AWS resources and applications in real-time.
- Collects and tracks metrics, logs, and events.

**CloudWatch Components:**

### CloudWatch Metrics

- Time-ordered data points (CPU, memory, disk, network, etc.)
- Default metrics for most AWS services
- Custom metrics for application-specific data
- Metric resolution: Standard (1 min) or High (1 sec)

**Common Metrics:**

- **EC2:** CPUUtilization, NetworkIn/Out, DiskReadOps
- **RDS:** DatabaseConnections, ReadLatency, WriteLatency
- **Lambda:** Invocations, Duration, Errors, Throttles
- **ELB:** RequestCount, TargetResponseTime, HealthyHostCount

### CloudWatch Logs

- Centralized log management
- Real-time monitoring and analysis
- Log retention policies
- Export to S3 or stream to other services

**Log Groups and Streams:**

```
Log Group: /aws/lambda/my-function
├── Log Stream: 2025/10/22/[$LATEST]abc123
├── Log Stream: 2025/10/22/[$LATEST]def456
└── Log Stream: 2025/10/22/[$LATEST]ghi789
```

### CloudWatch Alarms

- Automated actions based on metric thresholds
- States: OK, ALARM, INSUFFICIENT_DATA
- Actions: SNS notifications, Auto Scaling, EC2 actions

**Alarm Example:**

```json
{
  "AlarmName": "HighCPU",
  "MetricName": "CPUUtilization",
  "Threshold": 80,
  "ComparisonOperator": "GreaterThanThreshold",
  "EvaluationPeriods": 2,
  "Period": 300,
  "Statistic": "Average"
}
```

### CloudWatch Dashboards

- Customizable views of metrics and logs
- Share across teams
- Cross-region and cross-account views

---

## AWS X-Ray

- **AWS X-Ray** helps analyze and debug distributed applications.
- Trace requests across microservices.

**X-Ray Features:**

- Service map visualization
- Request tracing
- Performance bottleneck identification
- Error and exception analysis
- Annotations and metadata

**X-Ray Use Cases:**

- Microservices debugging
- Performance optimization
- Dependency analysis
- Error root cause analysis

---

## AWS CloudTrail

- **AWS CloudTrail** records AWS API calls for auditing and compliance.
- Who did what, when, and from where.

**CloudTrail Features:**

- Event history (90 days free)
- Trail creation for long-term storage
- Log file integrity validation
- Multi-region and multi-account logging
- Integration with CloudWatch Logs

**CloudTrail Event Types:**

- **Management Events:** Control plane operations (CreateInstance, DeleteBucket)
- **Data Events:** Data plane operations (GetObject, PutObject)
- **Insights Events:** Unusual API activity detection

---

## Monitoring Best Practices

### Metrics

- **Define KPIs:** Identify key performance indicators
- **Set baselines:** Understand normal behavior
- **Create alarms:** Proactive alerting
- **Use dashboards:** Visualize important metrics
- **Monitor costs:** Track spending patterns

### Logging

- **Centralize logs:** Use CloudWatch Logs
- **Structured logging:** Use JSON format
- **Log levels:** DEBUG, INFO, WARN, ERROR
- **Retention policies:** Balance cost and compliance
- **Log analysis:** Use CloudWatch Insights or Athena

### Alerting

- **Meaningful alerts:** Avoid alert fatigue
- **Actionable notifications:** Include context
- **Escalation policies:** Define who gets notified
- **Test alerts:** Verify notification delivery
- **Document runbooks:** Response procedures

---

## Observability

**Three Pillars of Observability:**

1. **Metrics:** What is happening (quantitative)
2. **Logs:** Detailed event records (qualitative)
3. **Traces:** Request flow through system (distributed)

**AWS Observability Services:**

- **CloudWatch:** Metrics and logs
- **X-Ray:** Distributed tracing
- **CloudTrail:** API audit logs
- **AWS Distro for OpenTelemetry:** Open-source observability

---

# **Hands-On Labs**

*Labs sẽ được cập nhật*

---

## Week 7 Summary (Partial)

3 ngày đầu tuần 7 đã giới thiệu:

✅ Serverless Computing với AWS Lambda  
✅ Container Services (ECS, EKS, ECR)  
✅ Monitoring & Logging (CloudWatch, X-Ray, CloudTrail)  

**Tổng kết 33 ngày làm việc (8/9 - 22/10/2025)**

Đã hoàn thành 6 tuần đầy đủ + 3 ngày của tuần 7, bao gồm:
- Cloud Fundamentals
- Networking
- Compute
- Storage
- Security
- Databases
- Serverless & Containers
- Monitoring

**Các tuần tiếp theo sẽ được cập nhật khi có nội dung mới!**
