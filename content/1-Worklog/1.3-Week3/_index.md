---
title: "Week 3 Worklog"
weight: 3
chapter: false
pre: "<b> 1.3. </b>"
date: 2025-09-30
status: "Done"
lang: "en"
translationKey: "week3-module3"
---

# Module 3

**Date:** September 30, 2025  
**Status:** Done  
**Week:** Week 3  

---

## Lecture Notes

## Compute VM on AWS

### Amazon Elastic Compute Cloud (EC2)

- **Amazon EC2** works like a virtual or physical server — offering **fast provisioning, scalable resources, and high flexibility**.  
- EC2 supports various workloads such as web hosting, applications, databases, authentication services, or any server-based task.  

**Instance Types**

- EC2 configurations are selected via predefined **Instance Types**, not arbitrary hardware choices.  
- Instance Type defines:
  - CPU ( Intel / AMD / ARM **Graviton 1-3** / GPU )
  - Memory  
  - Network performance  
  - Storage  

![EC2 Instance Types](/images/1-Worklog/Week3/image.png)

---

### Amazon Machine Images (AMI)

- **AMI (Amazon Machine Image)** is a template containing the OS, apps, and configuration required to launch an instance.  
- AMIs can be:
  - AWS-provided (Amazon Linux, Windows, Ubuntu …)  
  - From AWS Marketplace  
  - Custom AMI (created by user)  

**Benefits of Custom AMIs**
- Reduce launch and setup time.  
- Enable fast backup and restore.  
- Ensure environment consistency across instances.  

---

### Backup in EC2

- **AWS Backup** offers centralized backup for multiple AWS services including EC2.  
- **EBS Snapshot** is the primary backup method:
  - Point-in-time copy  
  - Incremental storage (block-level changes only)  
  - Stored in S3 (not directly accessible)  
- **AMI Backup** creates a full image of a running instance for restore or migration.  

![EC2 Backup](/images/1-Worklog/Week3/image%201.png)

---

### Key Pair

- **Key Pair** = Public + Private key for secure login to EC2.  
  - Public key stored on instance  
  - Private key kept by user for SSH (Linux) or RDP (Windows)  
- Provides password-less authentication for better security.  
- ⚠️ If the private key is lost, you cannot connect again — AWS does not keep copies.  

![EC2 Key Pair](/images/1-Worklog/Week3/image%202.png)

---

### Amazon Elastic Block Store (EBS)

- **EBS** = block-level storage that can attach to EC2 instances.  
- Storage types:
  - **General Purpose SSD (gp2/gp3)** – balanced cost/performance  
  - **Provisioned IOPS SSD (io1/io2)** – for high IOPS apps  
  - **Throughput-Optimized HDD (st1)** – for large streaming workloads  
  - **Cold HDD (sc1)** – low-cost infrequent access  

**Key features**
- Detachable/reattachable volumes  
- Persistent data even when instance stops  
- Snapshot support for backup or migration  
- Auto-replication within an AZ  

![EBS Overview](/images/1-Worklog/Week3/image%203.png)

---

### Instance Store

- **Instance Store** = ephemeral storage physically attached to the host machine.  
**Characteristics**
- Very high I/O performance  
- Data lost on stop/terminate  
- Not detachable or snapshot-capable  

**Use cases**
- Temporary cache  
- Intermediate data processing  
- Workloads with external redundancy or replication  

Instance Store is free for certain Instance Types, size depends on type.

![Instance Store](/images/1-Worklog/Week3/image%204.png)

---

## User Data

- **User Data** = script executed once at instance launch (from AMI).  
- Linux → bash script; Windows → PowerShell.  

![EC2 User Data](/images/1-Worklog/Week3/image%205.png)

---

## Meta Data

- **EC2 Metadata** provides info about the instance itself (private/public IP, hostname, security groups, etc.).  
- Commonly used to auto-configure hostname or environment on boot via User Data.  

![EC2 Metadata](/images/1-Worklog/Week3/image%206.png)

---

## Amazon EC2 Auto Scaling

- **EC2 Auto Scaling** automatically adjusts instance count based on load.  

**Benefits**
- Elastic response to demand  
- Improved availability  
- Cost optimization  

**Components**
- **Auto Scaling Group (ASG)** – logical group of instances  
- **Launch Template/Config** – blueprint for new instances  
- **Scaling Policies** – rules for up/down scaling  

### Scaling Policy Types

- **Simple / Step Scaling:** Adjust by a fixed count using CloudWatch thresholds  
- **Target Tracking:** Keep a metric at a specific value (e.g., CPU 50%)  
- **Scheduled Scaling:** Predefined time-based changes  
- **Predictive Scaling:** Machine-Learning forecasting  

**Integration with Load Balancer**
- ASG works with ELB to distribute traffic.  
- New instances auto-register with the LB; terminated ones are deregistered.  

---

## Pricing Options

Four main models:  
1. **On-Demand** – pay per use (hour/second), most expensive, ideal for ≤6 h/day workloads.  
2. **Reserved Instances** – 1–3 year commitment for discounts; tied to type/family.  
3. **Savings Plans** – 1–3 year commitment discount without type restriction.  
4. **Spot Instances** – use spare capacity cheaply; may terminate with 2-min notice.  

→ Mix multiple pricing options within Auto Scaling Groups.  

---

## Amazon Lightsail

- **Lightsail** = low-cost compute service (starting at $3.5/mo).  
- Each instance includes a bundled data transfer quota cheaper than EC2.  
- Best for lightweight or dev/test workloads with < 2 h/day CPU spikes.  
- Supports snapshot backups like EC2.  
- Runs in a special VPC that can peer with standard VPCs in one click.  

---

## Amazon EFS (Elastic File System)

- **EFS** creates shared **NFSv4 network volumes** mountable by multiple EC2 instances simultaneously.  
- Scales to petabytes and supports only Linux.  
- Billed by used capacity (while EBS charges allocated size).  
- Can be mounted on-prem via Direct Connect or VPN.  

---

## Amazon FSx

- **Amazon FSx** provides fully managed file systems (Windows, Lustre, HPC).  
- AWS handles setup, backup, and scaling.  
- Accessible from EC2, on-prem, or users via NFS/SMB mounts.  

---

## AWS Application Migration Service (MGN)

- **MGN** migrates and replicates on-prem or virtual servers to AWS for disaster recovery.  
- Continuously replicates source servers to lightweight staging instances on EC2.  
- During cut-over, MGN automatically launches full-sized target EC2 instances.  
