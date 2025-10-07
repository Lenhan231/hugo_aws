---
title: "Week 2 Worklog"
weight: 2
chapter: false
pre: "<b> 1.2. </b>"
---

**Date:** 2025-09-16  
**Status:** "Done"  
**Week:** "Week 2"  

---

# **Lecture Notes**

## Networking Services on AWS

### Amazon Virtual Private Cloud (VPC)

- Amazon Virtual Private Cloud (Amazon VPC) allows you to launch AWS resources into a virtual network you define.

![image](/images/1-Worklog/Week2/c7387b7b-422f-41ee-a0b7-886017264e85.png)

- A VPC exists within a single Region. When creating a VPC, you must define an IPv4 CIDR block (required) and optionally an IPv6 one.  
- The default limit is 5 VPCs per Region per Account.  
- Commonly used to separate environments such as Production, Development, and Staging.  
- To achieve full resource isolation, use separate AWS Accounts rather than multiple VPCs.

![image](/images/1-Worklog/Week2/image.png)

### Subnets

- A subnet resides within one Availability Zone.  
- The subnet CIDR must be a subset of the parent VPC’s CIDR block.  
- AWS reserves 5 IP addresses in each subnet: network, broadcast, router, DNS, and future use.  

![image](/images/1-Worklog/Week2/image%201.png)

- A route table defines how traffic is directed.  
- Each VPC has a default route table containing only a local route allowing internal communication between subnets.  
- Custom route tables can be created, but the local route cannot be deleted.

![image](/images/1-Worklog/Week2/f02d3571-7a7c-4388-a88f-87929790a60b.png)

### Elastic Network Interface (ENI)

- An ENI is a virtual network card that can be moved between EC2 instances.  
- It retains its private IP, Elastic IP address, and MAC address when reassigned.  
- Elastic IP (EIP) is a static public IPv4 address that can be associated with an ENI.  
- Unused EIPs incur charges.

![image](/images/1-Worklog/Week2/image%202.png)

- A **VPC Endpoint** enables private connectivity to supported AWS services via **AWS PrivateLink** without using the public Internet.  
- Two types:  
  - **Interface Endpoint:** Uses an ENI with a private IP.  
  - **Gateway Endpoint:** Uses route tables (available for S3 and DynamoDB only).

![image](/images/1-Worklog/Week2/image%203.png)

### Internet Gateway (IGW)

- A horizontally scalable, redundant, and highly available VPC component that allows communication between instances in your VPC and the Internet.  
- Fully managed by AWS – no manual scaling or HA configuration required.

![image](/images/1-Worklog/Week2/image%204.png)
![image](/images/1-Worklog/Week2/image%205.png)

### NAT Gateway

- Allows instances in private subnets to access the Internet or other AWS services, but prevents inbound Internet connections.

![image](/images/1-Worklog/Week2/image%206.png)

### Security Group (SG)

- A **stateful** virtual firewall that controls inbound and outbound traffic to AWS resources.  
- Rules are based on protocol, port, source, or another security group.  
- Only allow rules are supported.  
- Applied to Elastic Network Interfaces (ENIs).

![image](/images/1-Worklog/Week2/image%207.png)
![image](/images/1-Worklog/Week2/image%208.png)

### Network Access Control List (NACL)

- A **stateless** virtual firewall that operates at the subnet level.  
- Rules control inbound and outbound traffic by protocol, port, and source.  
- Default NACL allows all traffic.

![image](/images/1-Worklog/Week2/image%209.png)
![image](/images/1-Worklog/Week2/image%2010.png)

### VPC Flow Logs

- Capture metadata about IP traffic to and from network interfaces in your VPC.  
- Logs can be delivered to Amazon CloudWatch Logs or S3.  
- Flow Logs do not record packet payloads.

![image](/images/1-Worklog/Week2/image%2011.png)

## VPC Peering & Transit Gateway

### VPC Peering

- Enables direct, private connectivity between two VPCs without traversing the Internet.  
- Does not support transitive routing or overlapping CIDRs.

![image](/images/1-Worklog/Week2/image%2012.png)
![image](/images/1-Worklog/Week2/image%2013.png)
![image](/images/1-Worklog/Week2/image%2014.png)

### AWS Transit Gateway (TGW)

- Acts as a hub to connect multiple VPCs and on-prem networks, simplifying complex mesh topologies.  
- TGW Attachments associate subnets in specific AZs with a TGW.  
- All subnets within the same AZ can reach the TGW once attached.

![image](/images/1-Worklog/Week2/image%2015.png)
![image](/images/1-Worklog/Week2/image%2016.png)

## VPN & Direct Connect

### Site-to-Site VPN

- Establishes a secure IPSec connection between an on-premises data center and AWS VPC.  
- Consists of:  
  - **Virtual Private Gateway (VGW):** AWS-managed, multi-AZ endpoints.  
  - **Customer Gateway (CGW):** Customer-managed device or software appliance.

### AWS Direct Connect

- Provides a dedicated private network connection between an on-prem data center and AWS.  
- Typical latency: 20–30 ms.  
- In Vietnam, available through Hosted Connections (via partners).  
- Bandwidth is adjustable.

## Elastic Load Balancing (ELB)

### Overview

- A fully managed service distributing traffic across multiple targets (EC2, containers, etc.).  
- Supports HTTP, HTTPS, TCP, TLS.  
- Can be deployed in public or private subnets.  
- Provides DNS names; only NLB supports static IPs.  
- Includes health checks and access logs (to S3).  
- Supports sticky sessions (session affinity).  
- Types: **Application**, **Network**, **Classic**, and **Gateway** Load Balancer.

### Application Load Balancer (ALB)

- Operates at Layer 7 (HTTP/HTTPS).  
- Supports path-based routing (e.g., /mobile vs /desktop).  
- Targets: EC2, Lambda, IP addresses, containers (ECS/EKS).

![image](/images/1-Worklog/Week2/image%2017.png)

### Network Load Balancer (NLB)

- Operates at Layer 4 (TCP/TLS).  
- Supports static IPs and handles millions of requests per second.  
- Targets: EC2, IP addresses, containers (ECS/EKS).

### Gateway Load Balancer (GWLB)

- Operates at Layer 3 (IP packets).  
- Uses the GENEVE protocol on port 6081.  
- Routes traffic to virtual appliances such as firewalls or monitoring tools.  
- Partner list: [aws.amazon.com/elasticloadbalancing/partners](https://aws.amazon.com/elasticloadbalancing/partners)

Typical GWLB architecture:

![image](/images/1-Worklog/Week2/image%2018.png)

---

# **Exploration**

## [AWS Advanced Networking – Specialty Study Guide](https://www.amazon.co.uk/Certified-Advanced-Networking-Official-Study/dp/1119439833)

- Official study guide covering exam topics, AWS network design principles, and real-world architecture scenarios.

---

# **Hands-On Labs**

## Lab 03 – Amazon VPC & Networking Basics
1. Create VPC → *03-03.1*  
2. Create Subnet → *03-03.2*  
3. Create Internet Gateway (IGW) → *03-03.3*  
4. Create Route Table (Outbound via IGW) → *03-03.4*  
5. Create Security Groups → *03-03.5*  
6. Launch EC2 Instances in Subnets → *04-1*  
7. Test Connection Between Instances → *04-2*  
8. Create NAT Gateway (Private ↔ Internet) → *04-3*  
9. EC2 Instance Connect Endpoint (no bastion) → *04-5*

---

## Lab 10 – Hybrid DNS (Route 53 Resolver)
1. Generate Key Pair → *10-02.1*  
2. Initialize CloudFormation Template → *10-02.2*  
3. Configure Security Group → *10-02.3*  
4. Set up DNS System → *10-05*  
5. Create Route 53 Outbound Endpoint → *10-05.1*  
6. Create Resolver Rules → *10-05.2*  
7. Create Inbound Endpoints → *10-05.3*

---

## Lab 19 – VPC Peering
1. Initialize CloudFormation Templates → *19-02.1*  
2. Create Security Group → *19-02.2*  
3. Create EC2 Instance (Test Peering) → *19-02.3*  
4. Create Peering Connection → *19-04*  
5. Configure Route Tables (Cross-VPC) → *19-05*  
6. Enable Cross-Peer DNS → *19-06*

---

## Lab 20 – AWS Transit Gateway
1. Preparation Steps → *20-02*  
2. Create Transit Gateway → *20-03*  
3. Create TGW Attachments → *20-04*  
4. Create TGW Route Tables → *20-05*  
5. Add TGW Routes to VPC Route Tables → *20-06*
