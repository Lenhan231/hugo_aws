---
title: "Week 6 Worklog"
weight: 6
chapter: false
pre: "<b> 1.6. </b>"
---

**Date:** 2025-10-07  
**Status:** "Done"  
**Week:** "Week 6"  

---

# **Lecture Notes**

## Database Concepts Review

- A **database** is an organized (or semi-structured) collection of information stored on storage devices to support concurrent access by multiple users or programs with different goals.

### Sessions
- A **session** spans from the moment a client connects to the DBMS until the connection is terminated.

### Primary Key
- A **primary key** uniquely identifies each row in a relational table.

### Foreign Key
- A **foreign key** in one table references the **primary key** of another table, creating a relationship between them.

![image](/images/1-Worklog/Week6/image.png)

### Index
- An **index** accelerates data retrieval at the cost of extra writes and storage to maintain the index structure.
- Indexes locate data without scanning every row; they can be defined over one or more columns.

![image](/images/1-Worklog/Week6/image%201.png)

### Partitioning
- **Partitioning** splits a large table into smaller, independent pieces (partitions), potentially placed on different storage.
- Benefits: better query performance, easier maintenance, and scalability.
- Common types:
  - **Range** (e.g., by date)
  - **List**
  - **Hash**
  - **Composite** (combination)

### Execution Plan / Query Plan
- A **query plan** details how the DBMS will execute an SQL statement (access paths, joins, sorts).
- Types:
  - **Estimated plan** (before execution)
  - **Actual plan** (from executed query)
- Key operators: table scan, index seek/scan, nested loops, hash/merge join, sort, aggregate, filter.

![image](/images/1-Worklog/Week6/image%202.png)

### Database Logs
- **Database logs** record all changes (INSERT/UPDATE/DELETE) and operations.
- Typical log types: **transaction**, **redo**, **undo**, **binary** logs.
- Uses: recovery, integrity, consistency/durability (ACID), replication, performance analysis.

### Buffers
- A **buffer pool** caches pages read from disk to minimize I/O.
- Management strategies:
  - Replacement: LRU, FIFO, Clock
  - Write policies: immediate vs. deferred
  - **Prefetching** to warm the cache

![image](/images/1-Worklog/Week6/image%203.png)

### RDBMS
- An **RDBMS** stores data in related tables (rows/columns), enforces integrity constraints, uses SQL, and provides ACID guarantees.
- Popular engines: Oracle, MySQL, SQL Server, PostgreSQL, IBM Db2.

![image](/images/1-Worklog/Week6/image%204.png)

### NoSQL Overview
- **NoSQL** systems target un/semistructured data with high scalability and performance.
- Types:
  - **Document** (MongoDB, CouchDB)
  - **Key–Value** (Redis, DynamoDB)
  - **Column-Family** (Cassandra, HBase)
  - **Graph** (Neo4j, Amazon Neptune)
- Traits: schema flexibility, horizontal scaling, big-data friendliness, CAP-oriented designs.

![image](/images/1-Worklog/Week6/image%205.png)

#### RDBMS vs. NoSQL (high-level)
![image](/images/1-Worklog/Week6/image%206.png)

### OLTP vs. OLAP
- **OLTP**: many small, concurrent transactions; normalized data; short queries; index-heavy.
- **OLAP**: complex analytics over large historical datasets; star/snowflake schemas; read-heavy.

![image](/images/1-Worklog/Week6/image%207.png)

---

## Amazon RDS & Aurora

### Amazon Relational Database Service (RDS)
Managed relational databases that simplify provisioning, patching, backups, and HA.

- Supported engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, **Amazon Aurora**.
- Key features: automated backups/patching, easy scaling, Multi-AZ high availability, encryption & VPC/IAM/SSL security.
- Deployment options:
  - **Single-AZ**
  - **Multi-AZ** (synchronous standby in another AZ)
  - **Read Replicas** for scaling reads

![image](/images/1-Worklog/Week6/image%208.png)
![image](/images/1-Worklog/Week6/image%209.png)

### Amazon Aurora
Cloud-native, MySQL/PostgreSQL-compatible relational database re-architected for AWS.

![image](/images/1-Worklog/Week6/image%2010.png)

- Highlights:
  - Up to ~5× MySQL / ~3× PostgreSQL performance (typical benchmarks)
  - Storage auto-scales to 128 TB
  - Six-way replication across three AZs; self-healing storage
  - **Aurora Serverless** (on-demand capacity)
  - **Global Database** for low-latency multi-region

![image](/images/1-Worklog/Week6/image%2011.png)
![image](/images/1-Worklog/Week6/image%2012.png)

---

## Amazon Redshift

Fully managed cloud data warehouse optimized for large-scale analytics (OLAP).

- Columnar storage, compression, MPP execution; scales from hundreds of GB to PB.
- Integrations: S3, Kinesis, DynamoDB, BI tools; strong security features.
- Concurrency Scaling adds capacity automatically during spikes.
- Architecture: **cluster** (leader node + compute nodes), each compute node has **slices**.

Deployment options:
- **Redshift Provisioned**
- **Redshift Serverless**
- **Redshift Spectrum** (query S3 directly)

Use cases: enterprise BI, data lake analytics, dashboards, trend analysis, forecasting.

![image](/images/1-Worklog/Week6/image%2013.png)
![image](/images/1-Worklog/Week6/image%2014.png)

---

## Amazon ElastiCache

Managed in-memory caching service for **Redis** and **Memcached** to reduce latency and offload databases.

- Microsecond reads, Multi-AZ with failover, simple scaling, encryption/auth, automated ops.
- **Redis**: rich data structures, backups, replication, cluster mode.
- **Memcached**: simple, horizontally scalable cache with auto-discovery.

Common uses: web/mobile acceleration, DB query caching, session stores, leaderboards, pub/sub, queues.

---

# **Exploration**

## [The Data Warehouse Toolkit](https://www.amazon.com/Data-Warehouse-Toolkit-Definitive-Dimensional/dp/1118530802)
- Canonical reference for dimensional modeling and DW design patterns.

---

# **Hands-On Labs**

## Lab 05 – Amazon RDS & EC2 Integration
1. Create a VPC → 05-2.1  
2. Create EC2 Security Group → 05-2.2  
3. Create RDS Security Group → 05-2.3  
4. Create DB Subnet Group → 05-2.4  
5. Create EC2 Instance → 05-3  
6. Create RDS Database Instance → 05-4  
7. Application Deployment → 05-5  
8. Backup and Restore → 05-6  
9. Clean Up Resources → 05-7

## Lab 43 – AWS Database Migration Service (DMS)
1. EC2 Connect RDP Client → 43-01  
2. EC2 Connect Fleet Manager → 43-02  
3. SQL Server Source Config → 43-03  
4. Oracle Connect Source DB → 43-04  
5. Oracle Config Source DB → 43-05  
6. Drop Constraint → 43-06  
7. MSSQL → Aurora MySQL Target Config → 43-07  
8. MSSQL → Aurora MySQL Create Project → 43-08  
9. MSSQL → Aurora MySQL Schema Conversion → 43-09  
10. Oracle → MySQL Schema Conversion (1) → 43-10  
11. Create Migration Task & Endpoints → 43-11  
12. Inspect S3 → 43-12  
13. Create Serverless Migration → 43-13  
14. Create Event Notification → 43-14  
15. Logs → 43-15  
16. Troubleshoot: Memory Pressure → 43-16  
17. Troubleshoot: Table Error → 43-17
