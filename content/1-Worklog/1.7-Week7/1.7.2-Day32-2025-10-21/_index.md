---
title: "Day 32 - Container Services"
weight: 2
chapter: false
pre: "<b> 1.7.2. </b>"
---

**Date:** 2025-10-21 (Thứ Ba)  
**Status:** "Planned"  

---

# **Lecture Notes**

## Containers Overview

### What are Containers?

- **Containers** package application code with dependencies into a single unit.
- Lightweight, portable, and consistent across environments.
- Share the host OS kernel (unlike VMs).

**Benefits:**

- Consistent environments (dev, test, prod)
- Fast startup times
- Efficient resource utilization
- Easy scaling
- Microservices architecture support

---

## Docker Basics

**Docker Components:**

- **Image:** Read-only template with application and dependencies
- **Container:** Running instance of an image
- **Dockerfile:** Instructions to build an image
- **Registry:** Store and distribute images (Docker Hub, ECR)

**Basic Dockerfile:**

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

---

## Amazon Elastic Container Registry (ECR)

- **Amazon ECR** is a fully managed Docker container registry.
- Secure, scalable, and reliable.
- Integrated with ECS, EKS, and Lambda.

**ECR Features:**

- Private and public repositories
- Image scanning for vulnerabilities
- Lifecycle policies for image cleanup
- Cross-region and cross-account replication
- Encryption at rest
- IAM-based access control

---

## Amazon Elastic Container Service (ECS)

- **Amazon ECS** is a fully managed container orchestration service.
- Run and scale containerized applications.

**ECS Launch Types:**

### EC2 Launch Type

- You manage EC2 instances
- More control over infrastructure
- Use Reserved Instances for cost savings

### Fargate Launch Type

- Serverless compute for containers
- No EC2 instance management
- Pay only for resources used
- Automatic scaling

**ECS Components:**

- **Task Definition:** Blueprint for your application (like a Dockerfile for ECS)
- **Task:** Running instance of a task definition
- **Service:** Maintains desired number of tasks
- **Cluster:** Logical grouping of tasks/services

---

## Amazon Elastic Kubernetes Service (EKS)

- **Amazon EKS** is a managed Kubernetes service.
- Run Kubernetes without managing control plane.

**EKS Features:**

- Fully managed Kubernetes control plane
- Automatic version upgrades and patching
- Integration with AWS services (IAM, VPC, ELB)
- Multi-AZ for high availability
- Support for Fargate (serverless Kubernetes)

**EKS vs ECS:**

| Feature | ECS | EKS |
|---------|-----|-----|
| Orchestrator | AWS proprietary | Kubernetes |
| Learning Curve | Easier | Steeper |
| Portability | AWS-specific | Multi-cloud |
| Ecosystem | AWS services | Kubernetes ecosystem |
| Cost | Lower | Higher (control plane cost) |

---

## When to Use What?

**Use Lambda when:**

- Event-driven workloads
- Short-running tasks (< 15 min)
- Unpredictable traffic
- Minimal infrastructure management

**Use Containers (ECS/EKS) when:**

- Long-running applications
- Complex microservices
- Need specific runtime environments
- Existing containerized applications
- Multi-cloud portability (EKS)

---

# **Hands-On Labs**

*Labs sẽ được cập nhật*
