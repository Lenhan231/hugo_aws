---
title: "Week 5 Worklog"
weight: 5
chapter: false
pre: "<b> 1.5. </b>"
---

**Date:** 2025-10-07  
**Status:** "Done"  
**Week:** "Week 5"  

---

# **Lecture Notes**

## Security

### Shared Responsibility Model

- In cloud computing, security is a **shared responsibility** between the cloud provider and the customer.  
- Customers must securely configure services, apply best practices, and use security controls from the hypervisor exposure upward to application/data layers.

![image](/images/1-Worklog/Week5/image.png)

- The split of responsibilities varies by service model:
  - Infrastructure-level services
  - Partially managed services
  - Fully managed services

![image](/images/1-Worklog/Week5/image%201.png)

---

## AWS Identity and Access Management (IAM)

### Root Account

- Has unrestricted access to all AWS services/resources and can remove any attached permissions.  
- Best practices:
  - Create and use an **IAM Administrator** user for daily operations.
  - Lock away root credentials (dual control).  
  - Keep the root user’s email and domain valid and renewed.

### IAM Overview

- IAM controls access to AWS services and resources in your account.  
- **Principals** include: root user, IAM users, federated users, IAM roles, assumed-role sessions, AWS services, and anonymous users.  
- Notes:
  - IAM users are not separate AWS accounts.
  - New IAM users start with **no permissions**.
  - Grant permissions by attaching **policies** to users, groups, or roles.
  - Use **IAM groups** to manage many users (groups cannot be nested).

![image](/images/1-Worklog/Week5/image%202.png)

### IAM Policies

- JSON documents defining permissions.  
- Types:
  - **Identity-based policies** (attached to principals)
  - **Resource-based policies** (attached to resources)
- Evaluation rule: **explicit Deny** overrides Allow across all policies.

Pattern to constrain S3 administration:
- Allow all `s3:*` actions on a specific bucket.
- Explicitly **Deny** all non-S3 actions.

![image](/images/1-Worklog/Week5/image%203.png)

### IAM Roles

- Roles provide **temporary permissions** assumed by users, services, or external identities.  
- Common use cases:
  - Let an AWS service act on your behalf (e.g., EC2 → S3 writes)
  - Cross-account access
  - Federation from external IdPs
  - Credentials for apps on EC2 without storing access keys

![image](/images/1-Worklog/Week5/image%204.png)

**Benefits**
- No long-term credentials, short-lived sessions, least privilege, and easier large-scale access management.

![image](/images/1-Worklog/Week5/image%205.png)

---

## Amazon Cognito

- Managed authentication/authorization and user management for web & mobile apps.  
- Components:
  - **User Pools:** Sign-up/sign-in user directories.
  - **Identity Pools:** Federated identities for temporary AWS credentials to access services.

![image](/images/1-Worklog/Week5/image%206.png)
![image](/images/1-Worklog/Week5/image%207.png)
![image](/images/1-Worklog/Week5/image%208.png)
![image](/images/1-Worklog/Week5/image%209.png)

---

## AWS Organizations

- Centrally manage multiple AWS accounts in a single organization.

**Benefits**
- Centralized account management  
- **Consolidated Billing**  
- Hierarchies with **Organizational Units (OUs)**  
- Guardrails with **Service Control Policies (SCPs)**

### Organizational Units (OUs)
- Group accounts by department, project, or environment; nest OUs for hierarchical policies.

### Consolidated Billing
- One invoice for all accounts; volume pricing benefits; no extra cost.

### Service Control Policies (SCPs)
- Define **maximum permissions** for accounts; they **limit** but do not grant permissions.  
- Apply to accounts or OUs; affect all users/roles, **including root**; **Deny overrides Allow**.

**Example SCP (deny bucket deletion)**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    { "Effect": "Deny", "Action": ["s3:DeleteBucket"], "Resource": "*" }
  ]
}
````

![image](/images/1-Worklog/Week5/image%2010.png)

---

## AWS Identity Center (formerly AWS SSO)

* Centralizes access to AWS accounts and external applications.
* Identity sources: built-in, AWS Managed Microsoft AD, on-prem AD (trust/AD Connector), or external IdPs.
* **Permission Sets** define what users/groups can do in target accounts (materialized as IAM roles). Multiple permission sets per user are supported.

![image](/images/1-Worklog/Week5/image%2011.png)

---

## AWS Key Management Service (KMS)

* Managed keys for data protection with deep service integration and full auditability.

**Highlights**

* Create/manage keys without operating your own HSM infrastructure.
* Fine-grained access via IAM & key policies; usage logged in CloudTrail.

**Key categories**

* Customer-managed keys, AWS-managed keys, and AWS-owned keys.

![image](/images/1-Worklog/Week5/image%2012.png)

---

## AWS Security Hub

* Aggregates and prioritizes security findings and posture across accounts/services.

**Capabilities**

* Automated checks, normalized findings, prioritized remediation workflows.
* Compliance standards: CIS AWS Foundations, PCI DSS, AWS Foundational Security Best Practices.

**Integrations**

* GuardDuty, Inspector, Macie, Firewall Manager, IAM Access Analyzer, plus partner tools.

**Outcomes**

* Less time aggregating, more time fixing; unified visibility and improved security hygiene.

![image](/images/1-Worklog/Week5/image%2013.png)

---

# **Exploration**

## [AWS Certified Security – Specialty: All-in-One Exam Guide (SCS-C01)](https://dokumen.pub/aws-certified-security-specialty-all-in-one-exam-guide-exam-scs-c01-1260461734-9781260461725-1260461726.html)

* Comprehensive preparation material for the Security Specialty certification.

---

# **Hands-On Labs**

## Lab 18 – AWS Security Hub

1. Enable Security Hub → 18-02
2. Score for Each Set of Criteria → 18-03
3. Clean Up Resources → 18-04

## Lab 22 – AWS Lambda Automation with Slack

1. Create VPC → 22-2.1
2. Create Security Group → 22-2.2
3. Create EC2 Instance → 22-2.3
4. Incoming Webhooks (Slack) → 22-2.4
5. Create Tag for Instance → 22-3
6. Create Role for Lambda → 22-4
7. Function: Stop Instance → 22-5.1
8. Function: Start Instance → 22-5.2
9. Check Result → 22-6
10. Clean Up Resources → 22-7

## Lab 27 – AWS Resource Groups & Tagging

1. Create EC2 Instance with Tag → 27-2.1.1
2. Manage Tags in AWS Resources → 27-2.1.2
3. Filter Resources by Tag → 27-2.1.3
4. Use Tags with CLI → 27-2.2
5. Create a Resource Group → 27-3
6. Clean Up Resources → 27-4

## Lab 28 – IAM Cross-Region Role & Policy

1. Create IAM User → 28-2.1
2. Create IAM Policy → 28-3
3. Create IAM Role → 28-4
4. Switch Roles → 28-5.1
5. Access EC2 Console – Tokyo → 28-5.2.1
6. Access EC2 Console – N. Virginia → 28-5.2.2
7. Create EC2 (No Qualified Tags) → 28-5.2.3
8. Edit EC2 Resource Tag → 28-5.2.4
9. Policy Check → 28-5.2.5
10. Clean Up Resources → 28-6

## Lab 30 – IAM Restriction Policy

1. Create Restriction Policy → 30-3
2. Create IAM Limited User → 30-4
3. Test IAM User Limits → 30-5
4. Clean Up Resources → 30-6

## Lab 33 – AWS KMS & CloudTrail Integration

1. Create Policy and Role → 33-2.1
2. Create Group and User → 33-2.2
3. Create KMS Key → 33-3
4. Create S3 Bucket → 33-4.1
5. Upload Data to S3 → 33-4.2
6. Create CloudTrail → 33-5.1
7. Log to CloudTrail → 33-5.2
8. Create Amazon Athena → 33-5.3
9. Query with Athena → 33-5.4
10. Test & Share Encrypted S3 Data → 33-6
11. Resource Cleanup → 33-7

## Lab 44 – IAM Advanced Role Control

1. Create IAM Group → 44-2
2. Create IAM Users → 44-3.1
3. Check Permissions → 44-3.2
4. Create Admin IAM Role → 44-4.1
5. Configure Switch Role → 44-4.2
6. Restrict Switch Role by IP → 44-4.3.1
7. Restrict Switch Role by Time → 44-4.3.2
8. Clean Up Resources → 44-5

## Lab 48 – IAM Access Keys & Roles

1. Create EC2 Instance → 48-1.1
2. Create S3 Bucket → 48-1.2
3. Generate IAM User and Access Key → 48-2.1
4. Use Access Key → 48-2.2
5. Create IAM Role → 48-3.1
6. Use IAM Role → 48-3.2
7. Clean Up Resources → 48-4

```
```
