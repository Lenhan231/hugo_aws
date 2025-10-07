---
title: "Week 3 Worklog"
weight: 3
chapter: false
pre: "<b> 1.3. </b>"
---

**Date:** 2025-09-30  
**Status:** "Done"  
**Week:** "Week 3"  

---

# **Lecture Notes**

## Compute on AWS

### Amazon Elastic Compute Cloud (EC2)

- **Amazon EC2** provides resizable compute capacity in the cloud, similar to a virtual or physical server.  
- It supports workloads such as web hosting, applications, databases, authentication services, and other general-purpose server tasks.  

**Instance Types**
- EC2 configurations are defined by **instance types**, not custom hardware.  
- Each type specifies:
  - CPU (Intel, AMD, ARM – Graviton 1/2/3) / GPU
  - Memory  
  - Network  
  - Storage  

![image](/images/1-Worklog/Week3/image.png)

---

### Amazon Machine Images (AMI)

- **AMI (Amazon Machine Image)** is a template that defines the software configuration of an instance, including OS, apps, and settings.  
- Types of AMIs:
  - Provided by AWS (Amazon Linux, Windows, Ubuntu, etc.)  
  - AWS Marketplace AMIs  
  - Custom AMIs created by users  

**Benefits of Custom AMIs**
- Faster instance launch and setup  
- Simplified backup and restore  
- Consistent environment across multiple instances  

---

### Backup in EC2

- **AWS Backup** provides centralized backup for AWS services including EC2.  
- **EBS Snapshots** back up EBS volumes:
  - Point-in-time backups  
  - Incremental (stores only changed blocks)  
  - Stored in S3 (not directly accessible)  
- **AMI Backup** captures the full EC2 configuration as an image.

![image](/images/1-Worklog/Week3/image%201.png)

---

### Key Pair

- **Key Pairs** are used for secure authentication when connecting to EC2:
  - **Public Key** – stored on the instance  
  - **Private Key** – kept by the user for SSH (Linux) or RDP (Windows)  
- Replaces passwords for better security.  
- **Important:** If you lose your private key, AWS cannot recover it.

![image](/images/1-Worklog/Week3/image%202.png)

---

### Elastic Block Store (EBS)

- **Amazon EBS** provides persistent block storage for EC2 instances.  
- Volume types:
  - **General Purpose SSD (gp2/gp3)** – balance between performance & cost  
  - **Provisioned IOPS SSD (io1/io2)** – for high IOPS workloads  
  - **Throughput Optimized HDD (st1)** – for large, sequential data  
  - **Cold HDD (sc1)** – low-cost, infrequently accessed data  

**Key Features**
- Attach/detach volumes from instances  
- Data persists when instances stop  
- Create snapshots for backup or cross-region copy  
- Automatically replicated within an AZ  

![image](/images/1-Worklog/Week3/image%203.png)

---

### Instance Store

- **Instance Store** provides temporary block-level storage physically attached to the EC2 host.  

**Characteristics**
- Very high I/O and throughput  
- Data lost when instance stops or terminates  
- Cannot be detached or snapshotted  

**Use Cases**
- Caching or temporary data processing  
- Applications with their own redundancy or replication  

![image](/images/1-Worklog/Week3/image%204.png)

---

### User Data

- **User Data** scripts run automatically at instance launch (once per AMI provision).  
- Linux – bash scripts  
- Windows – PowerShell scripts  

![image](/images/1-Worklog/Week3/image%205.png)

---

### Metadata

- **EC2 Instance Metadata** provides details about the running instance such as private/public IP, hostname, and security groups.  
- Often used in user data scripts for dynamic configuration.

![image](/images/1-Worklog/Week3/image%206.png)

---

## Amazon EC2 Auto Scaling

- **EC2 Auto Scaling** automatically adjusts the number of EC2 instances based on demand.

**Benefits**
- Elastic capacity adjustment  
- Increased application availability  
- Cost optimization  

**Components**
- **Auto Scaling Group (ASG)** – logical group of EC2 instances  
- **Launch Template / Configuration** – defines instance parameters  
- **Scaling Policies** – rules for adding/removing instances  

### Scaling Policies
- **Simple / Step Scaling** – add/remove instances when thresholds are met  
- **Target Tracking** – maintain a metric (e.g., CPU = 50%)  
- **Scheduled Scaling** – scale on a predefined schedule  
- **Predictive Scaling** – uses ML to forecast and scale proactively  

### Integration with Load Balancer
- ASGs often pair with **Elastic Load Balancers (ELB)**.  
- New instances automatically register; terminated instances deregister automatically.

---

## EC2 Pricing Options

- **On-Demand:** Pay per hour/second. Most expensive but flexible.  
- **Reserved Instances:** 1- or 3-year commitment for discount; tied to specific instance type/family.  
- **Savings Plans:** 1- or 3-year commitment; flexible across instance families.  
- **Spot Instances:** Use spare capacity at up to 90% discount; can be terminated with 2-minute notice.  

> Combine multiple pricing models within an Auto Scaling Group for cost optimization.

---

## Amazon Lightsail

- Simplified compute service with predictable monthly pricing (starting ~$3.5/month).  
- Includes bundled data transfer at lower rates than EC2.  
- Ideal for small workloads, development, or testing environments.  
- Supports snapshots for backups.  
- Runs inside a managed VPC and can connect to standard VPCs via one-click peering.

---

## Amazon EFS (Elastic File System)

- Fully managed NFSv4 file system mountable by multiple EC2 instances simultaneously.  
- Scales automatically to petabytes.  
- Pay only for the storage used (unlike EBS provisioned size).  
- Can be mounted from on-prem via VPN or Direct Connect.

---

## Amazon FSx

- Managed, scalable file systems for Windows, Lustre, and NetApp ONTAP.  
- AWS handles setup, scaling, and backups.  
- Accessible from EC2, on-prem servers, or users via SMB or NFS protocols.

---

## AWS Application Migration Service (MGN)

- Used for migrating or replicating physical/virtual servers to AWS for DR or modernization.  
- Continuously replicates source machines to lightweight staging instances on EC2.  
- During cut-over, MGN launches fully functional EC2 instances from the replicated data.

---

# **Exploration**

## [Microsoft Workloads on AWS](https://youtube.com/playlist?list=PLhr1KZpdzukdJllxulUM7pMB7aJ2_FfTP&si=HMwzvhqsFFljKfnC)
- A curated series covering deployment, optimization, and best practices for running Microsoft workloads on AWS.

---

# **Hands-On Labs**

### Lab 01 – AWS Account & IAM Setup
1. Create AWS Account → *01-01*  
2. Setup Virtual MFA Device → *01-02*  
3. Create Admin Group and Admin User → *01-03*  
4. Account Authentication Support → *01-04*

### Lab 07 – AWS Budgets & Cost Management
1. Create Budget by Template → *07-01*  
2. Create Cost Budget Tutorial → *07-02*  
3. Create Usage Budget → *07-03*  
4. Create Reserved Instance Budget → *07-04*  
5. Create Savings Plans Budget → *07-05*  
6. Clean Up Budgets → *07-06*

### Lab 09 – AWS Support Plans
1. AWS Support Packages → *09-01*  
2. Types of Support Requests → *09-02*  
3. Change Support Package → *09-03*  
4. Manage Support Requests → *09-04*
