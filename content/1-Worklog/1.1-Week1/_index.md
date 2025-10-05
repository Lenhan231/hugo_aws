---
title: "Week 1 Worklog"
weight: 1
chapter: false
pre: "<b> 1.1. </b>"
date: 2025-09-09
status: "Done"
---

# Module 1

**Date:** September 9, 2025  
**Status:** Done  
**Week:** Week 1  

---

## Lecture Note

### What Is Cloud Computing?

- It is the distribution of IT resources over the Internet with a pay-as-you-go model.

#### Benefits of Cloud Computing

- Pay only for what you use, providing cost optimization.  
- Increase development speed by leveraging automation and vendor management.  
- Flexible — scale resources up or down as needed.  
- Expand your applications globally with ease.

#### AWS — What Makes It Different?

- AWS has been the leading cloud provider for 13 consecutive years (as of 2023).  
- AWS stands out through its long-term vision and customer-centric culture.  
- AWS pricing philosophy: over time, customers pay less for the same service, features, or resources.  
- AWS puts customer value at the core of its Leadership Principles.  

![AWS Overview](/images/1-Worklog/Week1/image.png)

---

### How to Start Your Cloud Journey

- There are many AWS courses available — you can self-study to become a cloud professional.  
- Sign up for an AWS account and experiment through the **Free Tier**.  
- Recommended learning platforms:
  - [Udemy](https://udemy.com/)
  - [Cloud Guru](https://cloudguru.com/)
- AWS Learning Path: [AWS Training Paths](https://aws.amazon.com/training/learning-paths)

---

## AWS Infrastructure

### AWS Data Centers

- A single data center can host tens of thousands of servers.  
- All AWS data centers use purpose-built hardware optimized for its workloads.

### Availability Zone (AZ)

- An AZ consists of one or more data centers.  
- AZs are designed for **fault isolation**, preventing simultaneous failure of multiple AZs.  
- AZs are connected through high-speed private links.  
- AWS recommends deploying workloads across at least two AZs.

### Region

- An AWS Region contains at least three AZs. There are over 25 Regions globally.  
- Regions are connected through AWS’s global backbone network.  
- Services and data are isolated per Region (except a few global services).

### Edge Locations

- Edge Locations are data centers designed to minimize latency.  
- Key AWS services operating at the edge:
  - CloudFront (CDN)  
  - Web Application Firewall (WAF)  
  - Route 53 (DNS Service)

---

## AWS Management Tools

### AWS Management Console — Root Login

- You can log in using a **root account** or an **IAM User** (a sub-account controlling access to resources).

### AWS Management Console — IAM User Login

- Logging in as an IAM User requires an **Account ID** (12-digit number) to identify the AWS account.

### AWS Management Console — Service Search

- After logging in, you can search for any AWS service.  
- Each service has its own management dashboard and feature set.

### AWS Management Console — Support Center

- On the top-right menu, select **Support → Support Center** to create and manage support cases.

![AWS Support](/images/1-Worklog/Week1/image%20copy%204.png)

---

### AWS Command Line Interface (CLI)

- The AWS CLI is an open-source tool for managing AWS services via command line.  
- It provides the same functionality as the web-based console, but through commands.

![AWS CLI](/images/1-Worklog/Week1/image%20copy%207.png)

---

### AWS Software Development Kit (SDK)

- The AWS SDK simplifies development by offering consistent libraries for multiple programming languages.  
- It manages API lifecycle tasks such as credentials, retries, data marshalling, serialization, and deserialization.

![AWS SDK](/images/1-Worklog/Week1/image%20copy%205.png)

---

## Cost Optimization on AWS

- Choose appropriate compute and storage configurations.  
- Use discounted payment models (Reserved Instances, Savings Plans, Spot).  
- Remove unused resources; schedule automatic start/stop for non-critical workloads.  
- Leverage **serverless** services.  
- Design architectures that meet requirements efficiently.  
- Set up and use **AWS Budgets**.  
- Manage costs by department/application with **cost allocation tags**.  
- Continuously monitor and optimize expenses.

**Cost Estimation Tool:** [AWS Calculator](https://calculator.aws/#/)  
Create estimates, share them with others, and view pricing differences by Region.

![AWS Calculator](/images/1-Worklog/Week1/image%20copy%206.png)

---

## Working with AWS Support

- AWS offers four main support plans:
  1. **Basic** (Explore)
  2. **Developer** (Test/Dev)
  3. **Business** (Production)
  4. **Enterprise** (Large Enterprise)
- You can temporarily upgrade your plan to handle urgent production issues faster.

---

## Hands-On Practice

### Create an AWS Account

1. Create a new AWS account.  
2. Enable **MFA** for the root user.  
3. Create an **Admin group** and assign users.  
4. Verify account information.

### Getting Started with AWS Budgets

1. Create a **Cost Budget**.  
2. Create a **Usage Budget**.  
3. Create a **Reservation Budget**.  
4. Create a **Savings Plans Budget**.

### Request Assistance through AWS Support

1. Understand AWS Support plans.  
2. Access the **Support Center**.  
3. Manage support cases.

---

## Additional Study

### AWS Well-Architected Framework

- Provides design principles and best practices for building systems in the cloud.  
- Use the framework by answering guided questions to assess architectural alignment with AWS best practices.  
- When using the **Well-Architected Tool**, AWS offers direct recommendations for improvement through the Console.

---
