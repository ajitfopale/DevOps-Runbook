# Amazon EBS Volume Types

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 30 Minutes

---

# Objective

After reading this document, you will understand:

- What EBS Volume Types are
- Different categories of EBS volumes
- SSD vs HDD
- gp3 vs gp2
- io1 vs io2
- st1 vs sc1
- Which volume to choose for different workloads
- Production examples
- Interview questions

---

# Introduction

Not every application needs the same type of storage.

Examples:

- Database needs very fast storage.
- Backup storage does not need high speed.
- Log files need good throughput.
- Operating System needs balanced performance.

To meet these different requirements, AWS provides multiple EBS volume types.

---

# What is an EBS Volume Type?

An EBS Volume Type defines:

- Storage technology
- Performance
- IOPS
- Throughput
- Latency
- Cost

Choosing the correct volume type improves both application performance and cost efficiency.

---

# Categories of EBS Volumes

Amazon EBS provides two major categories.

```
Amazon EBS

│

├── SSD Volumes

│      ├── gp3

│      ├── gp2

│      ├── io1

│      └── io2

│

└── HDD Volumes

       ├── st1

       └── sc1
```

---

# SSD Volumes

SSD volumes provide:

- Low latency
- High IOPS
- Fast response time

Suitable for:

- Operating Systems
- Databases
- Boot volumes
- Enterprise applications

---

# 1. General Purpose SSD (gp3)

The most recommended EBS volume.

### Features

- Latest generation
- High performance
- Cost effective
- Independent IOPS
- Independent Throughput

### Default Performance

- 3,000 IOPS
- 125 MB/s Throughput

Can increase performance without increasing storage size.

### Suitable For

- Linux Servers
- Windows Servers
- Web Applications
- Application Servers
- Small Databases
- Production Workloads

### Production Example

```
Apache Server

↓

EC2

↓

gp3 Volume

↓

Website Files
```

---

# 2. General Purpose SSD (gp2)

Older generation SSD.

Performance depends on volume size.

Example:

```
Small Volume

↓

Lower IOPS

Large Volume

↓

Higher IOPS
```

AWS now recommends gp3 for new workloads.

---

# gp2 vs gp3

| Feature | gp2 | gp3 |
|----------|-----|-----|
| Generation | Older | Latest |
| Cost | Higher | Lower |
| Performance | Depends on Size | Independent |
| Default IOPS | Variable | 3,000 |
| Throughput | Lower | Higher |
| Recommended | No | Yes |

---

# 3. Provisioned IOPS SSD (io1)

Designed for applications requiring consistently high IOPS.

Examples:

- Oracle Database
- SQL Server
- SAP
- Banking Systems

Advantages

- Very high IOPS
- Low latency
- Predictable performance

Disadvantages

Higher cost.

---

# 4. Provisioned IOPS SSD (io2)

Latest generation of Provisioned IOPS.

Advantages over io1:

- Better durability
- Better reliability
- Higher performance
- Longer life

Recommended for mission-critical applications.

---

# io1 vs io2

| Feature | io1 | io2 |
|----------|-----|-----|
| Generation | Older | Latest |
| Durability | Good | Better |
| Reliability | High | Higher |
| Performance | High | Higher |
| Production | Supported | Recommended |

---

# HDD Volumes

HDD volumes focus on throughput rather than IOPS.

Suitable for:

- Large files
- Data processing
- Backup
- Log storage

---

# 5. Throughput Optimized HDD (st1)

Designed for workloads that require high throughput.

Examples:

- Big Data
- Hadoop
- Log Processing
- Data Warehousing

Not recommended for boot volumes.

---

# 6. Cold HDD (sc1)

Lowest cost EBS storage.

Used for:

- Archived data
- Backups
- Rarely accessed files

Performance is much lower than SSD.

---

# st1 vs sc1

| Feature | st1 | sc1 |
|----------|------|------|
| Speed | Faster | Slower |
| Cost | Moderate | Lowest |
| Use | Big Data | Archive |
| Throughput | Higher | Lower |

---

# Complete Comparison

| Volume | Storage | Best For |
|----------|----------|----------------|
| gp3 | SSD | General workloads |
| gp2 | SSD | Older workloads |
| io1 | SSD | High IOPS databases |
| io2 | SSD | Mission-critical databases |
| st1 | HDD | Big Data |
| sc1 | HDD | Archive |

---

# Which Volume Should I Choose?

| Workload | Recommended Volume |
|------------|-------------------|
| Linux Server | gp3 |
| Windows Server | gp3 |
| Apache | gp3 |
| Nginx | gp3 |
| Tomcat | gp3 |
| Jenkins | gp3 |
| Kubernetes Node | gp3 |
| MySQL | io2 |
| PostgreSQL | io2 |
| Oracle | io2 |
| SQL Server | io2 |
| Hadoop | st1 |
| Log Storage | st1 |
| Backup | sc1 |
| Archive | sc1 |

---

# Real Production Example

Imagine an E-commerce Application.

```
Customer

↓

Load Balancer

↓

EC2

↓

gp3 Volume

↓

Application Files

↓

RDS Database

↓

io2 Storage

↓

Daily Backup

↓

sc1 Archive
```

Different storage types are selected based on workload requirements.

---

# Best Practices

✔ Use gp3 for most workloads.

✔ Use io2 for production databases.

✔ Avoid gp2 for new deployments.

✔ Monitor IOPS using CloudWatch.

✔ Delete unused volumes.

✔ Encrypt production volumes.

✔ Schedule snapshots.

---

# Common Mistakes

❌ Choosing io2 for a simple website.

❌ Using sc1 for databases.

❌ Paying for unnecessary IOPS.

❌ Using HDD for operating systems.

❌ Ignoring throughput requirements.

---

# Interview Questions

## Beginner

1. What are EBS Volume Types?

2. How many types of EBS volumes exist?

3. What is gp3?

4. What is the difference between SSD and HDD?

5. Which EBS volume is cheapest?

---

## Intermediate

1. gp2 vs gp3?

2. io1 vs io2?

3. Which volume is best for MySQL?

4. Which volume is used for backups?

5. Which EBS volume should you choose for production?

---

# Quick Revision

```
SSD

↓

gp3

gp2

io1

io2

HDD

↓

st1

sc1
```

Remember:

- gp3 → Most workloads
- io2 → Database
- st1 → Big Data
- sc1 → Archive

---

# Summary

Amazon EBS provides multiple storage options to match different application requirements.

- **gp3** is the default choice for most workloads because it offers excellent performance at a lower cost.
- **io2** is designed for mission-critical databases that require consistently high IOPS and durability.
- **st1** is optimized for throughput-heavy workloads such as analytics and log processing.
- **sc1** is intended for low-cost archival storage.

Selecting the appropriate EBS volume type improves application performance while controlling storage costs.

---

# Related Topics

- EBS Snapshots
- EC2
- CloudWatch
- AWS Backup
- EBS Encryption

---

# References

- AWS EBS Volume Types Documentation: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html