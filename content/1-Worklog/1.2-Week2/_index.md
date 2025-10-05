---
title: "Week 2 Worklog"
weight: 2
chapter: false
pre: "<b> 1.2. </b>"
date: 2025-09-16
status: "Done"
lang: "en"
translationKey: "week2-module2"
---

# Module 2

**Date:** September 16, 2025  
**Status:** Done  
**Week:** Week 2  

---

## Lecture Notes

# Networking Services on AWS

## Amazon Virtual Private Cloud (VPC)

- Amazon Virtual Private Cloud (Amazon VPC) allows you to launch AWS resources into a virtual network you define.

![VPC Overview](/images/1-Worklog/Week2/c7387b7b-422f-41ee-a0b7-886017264e85.png)

- A VPC exists within a single **Region**. When creating a VPC, you must define an **IPv4 CIDR block** (mandatory) and optionally an IPv6 one.  
- The current limit is **5 VPCs per Region per Account**.  
- Commonly used to separate environments such as **Production / Dev / Test / Staging**.  
- Note: To completely isolate resources (so one user cannot view another’s), create **multiple AWS Accounts** — separate VPCs alone won’t achieve that.

![VPC Architecture](/images/1-Worklog/Week2/image.png)

### Subnet

- You can divide your VPC into multiple **subnets**.  
- Each subnet resides in a **specific Availability Zone (AZ)**.  
- Subnet CIDR must be a **subset** of the VPC CIDR block.  
- AWS reserves **five IP addresses** in each subnet (e.g. `10.10.1.0/24`):
  - Network address (10.10.1.0)
  - Broadcast address (10.10.1.255)
  - Router address (10.10.1.1)
  - DNS address (10.10.1.2)
  - Future use (10.10.1.3)

![Subnet Reserved IPs](/images/1-Worklog/Week2/image%201.png)

- **Route Tables** define routing paths within your VPC.  
- A default route table is automatically created (cannot be deleted) with one **“local”** route allowing communication between subnets in the same VPC.  
- Each subnet is associated with a route table. You can create **custom route tables**, but you cannot remove the **local route**.

![Route Table Example](/images/1-Worklog/Week2/f02d3571-7a7c-4388-a88f-87929790a60b.png)

### Elastic Network Interface (ENI)

- **ENI** is a virtual network card that can be attached or moved between EC2 instances.  
- When moved, it retains:
  - Private IP
  - Elastic IP (if assigned)
  - MAC address  
- **Elastic IP (EIP)** is a static public IPv4 address; unused EIPs incur charges.

![ENI and EIP](/images/1-Worklog/Week2/image%202.png)

- **VPC Endpoints** allow private connections between your VPC and supported AWS services via **AWS PrivateLink**, avoiding the public Internet.  
- Two types:
  - **Interface Endpoint:** Uses an ENI with a private IP in your VPC.  
  - **Gateway Endpoint:** Uses a route table entry (for **S3** and **DynamoDB** only).

![VPC Endpoints](/images/1-Worklog/Week2/image%203.png)

### Internet Gateway

- **Internet Gateway (IGW)** enables instances in a VPC to communicate with the Internet.  
- IGW is fully managed by AWS — no manual scaling or HA setup required.

![Internet Gateway Public Subnet](/images/1-Worklog/Week2/image%204.png)
![Internet Gateway Traffic Flow](/images/1-Worklog/Week2/image%205.png)

### NAT Gateway

- **NAT Gateway** allows instances in private subnets to **initiate outbound connections** to the Internet or other AWS services, but blocks inbound traffic.

![NAT Gateway](/images/1-Worklog/Week2/image%206.png)

### Security Group (SG)

- A **Security Group** is a **stateful firewall** controlling inbound and outbound traffic for AWS resources.  
- Rules can be based on **protocol**, **port**, **source address**, or **another SG**.  
- Only **allow rules** are supported — no explicit deny.  
- SGs apply at the **ENI level**.

![Security Group Overview](/images/1-Worklog/Week2/image%207.png)
![Security Group Example Rules](/images/1-Worklog/Week2/image%208.png)

### Network Access Control List (NACL)

- A **NACL** is a **stateless** firewall at the subnet level.  
- Filters traffic by **protocol**, **port**, and **source/destination address**.  
- Default NACL allows all inbound and outbound traffic.

![NACL Overview](/images/1-Worklog/Week2/image%209.png)
![NACL Example Rules](/images/1-Worklog/Week2/image%2010.png)

### VPC Flow Logs

- **VPC Flow Logs** capture **IP traffic metadata** (not packet content) for network interfaces in your VPC.  
- Logs can be delivered to **CloudWatch Logs** or **S3**.

![VPC Flow Logs](/images/1-Worklog/Week2/image%2011.png)

---

## VPC Peering & Transit Gateway

### VPC Peering

- **VPC Peering** connects two VPCs directly so resources can communicate privately without traversing the Internet.  
- One-to-one connection (**no transitive routing**).  
- Peering is not supported if VPCs have **overlapping CIDRs**.

![VPC Peering Diagram](/images/1-Worklog/Week2/image%2012.png)
![VPC Peering Routes](/images/1-Worklog/Week2/image%2013.png)
![VPC Peering Limitations](/images/1-Worklog/Week2/image%2014.png)

### Transit Gateway (TGW)

- **Transit Gateway** acts as a **central hub** to interconnect multiple VPCs and on-premises networks, simplifying complex routing.  
- **Transit Gateway Attachment** links subnets from each VPC to the TGW at the **AZ level**.  
- Once a subnet in an AZ is attached to the TGW, other subnets in the same AZ can use that connection.

![Transit Gateway Architecture](/images/1-Worklog/Week2/image%2015.png)
![Transit Gateway Flow](/images/1-Worklog/Week2/image%2016.png)

---

## VPN & Direct Connect

### VPN Site-to-Site

- Used in **hybrid architectures** to connect on-premises data centers with AWS VPCs.  
- Two endpoints:
  - **Virtual Private Gateway (VGW):** AWS-managed, redundant across AZs.  
  - **Customer Gateway (CGW):** On-premises endpoint (hardware or software).

### AWS Direct Connect

- **Direct Connect** provides a **dedicated private connection** between a customer’s data center and AWS.  
- Typical latency: **20–30 ms**.  
- In Vietnam, connections are made via **AWS Direct Connect Partners (Hosted Connections)**.  
- Bandwidth can be adjusted up or down as needed.

---

## Elastic Load Balancing (ELB)

### Definition

- **Elastic Load Balancing** distributes incoming traffic across multiple **EC2 instances**, **containers**, or **Lambda functions**.  
- Supports **HTTP/HTTPS, TCP/TLS** protocols.  
- Works in both **public** and **private** subnets.  
- Each ELB gets a **DNS name** (NLB supports **static IPs**).  
- Performs **health checks** to exclude unhealthy targets.  
- Four types:
  1. **Application Load Balancer (ALB)** – Layer 7  
  2. **Network Load Balancer (NLB)** – Layer 4  
  3. **Gateway Load Balancer (GWLB)** – Layer 3  
  4. **Classic Load Balancer (CLB)** – Legacy  
- **Sticky sessions (session affinity)** ensure that user requests during a session are routed to the same target (available on ALB/NLB/CLB).  
- **Access logs** can be enabled and stored in **S3** for analysis and troubleshooting.

### Application Load Balancer (ALB)

- Operates at **Layer 7** using **HTTP/HTTPS**.  
- Supports **path-based routing** (e.g. `/mobile`, `/desktop`).  
- Routes traffic to **IP addresses**, **EC2**, **Lambda**, or **Containers (ECS/EKS)** — even outside the VPC.

![Application Load Balancer](/images/1-Worklog/Week2/image%2017.png)

### Network Load Balancer (NLB)

- Operates at **Layer 4** using **TCP/TLS**.  
- Supports **static IPs** and can handle **millions of requests per second**.  
- Routes traffic to **external IPs**, **EC2**, or **Containers (ECS/EKS)**.

### Gateway Load Balancer (GWLB)

- Operates at **Layer 3**, listening to IP packets and forwarding them to target groups.  
- Uses **GENEVE protocol (port 6081)**.  
- Routes traffic to **virtual appliances** from AWS partners.  
- Supported vendors: <https://aws.amazon.com/elasticloadbalancing/partners/>

Typical GWLB deployment model:

![Gateway Load Balancer](/images/1-Worklog/Week2/image%2018.png)
