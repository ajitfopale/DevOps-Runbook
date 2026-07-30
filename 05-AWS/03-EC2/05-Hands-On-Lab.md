# Amazon EC2 Instance Types

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Beginner to Advanced  
> **Estimated Reading Time:** 45 Minutes

---

# Objective

After reading this document, you will understand:

- What EC2 Instance Types are
- How AWS names instance types
- Different instance families
- Which instance type to choose for different workloads
- ARM vs x86 processors
- Nitro System
- Real production examples
- Interview questions

---

# Introduction

Every application has different compute requirements.

Examples:

- A small website needs low CPU and memory.
- A database server needs large memory.
- A video rendering application needs a GPU.
- A data analytics application needs high-speed storage.

To support these workloads, AWS provides multiple EC2 instance types.

---

# What is an EC2 Instance Type?

An EC2 Instance Type defines the hardware configuration of a virtual server.

It determines:

- Number of vCPUs
- RAM (Memory)
- Storage options
- Network bandwidth
- EBS bandwidth
- Processor architecture

Example:

```
EC2 Instance

↓

2 vCPU

↓

4 GB RAM

↓

EBS Storage

↓

10 Gbps Network
```

---

# EC2 Instance Naming Convention

Example:

```
m7g.large
```

Breakdown:

| Part | Meaning |
|------|---------|
| m | Instance Family |
| 7 | Generation |
| g | Processor Type (Graviton) |
| large | Instance Size |

Another example:

```
c6i.xlarge
```

| Part | Meaning |
|------|---------|
| c | Compute Optimized |
| 6 | Generation |
| i | Intel Processor |
| xlarge | Instance Size |

---

# EC2 Instance Families

AWS groups instances into different families based on workload.

```
EC2

│

├── General Purpose

├── Compute Optimized

├── Memory Optimized

├── Storage Optimized

├── Accelerated Computing

└── HPC
```

---

# 1. General Purpose

Balanced CPU, memory, and networking.

Families:

- T
- M

Suitable for:

- Web Servers
- Small Databases
- Development
- Application Servers

Example:

```
Apache

↓

t3.micro
```

---

## T Family (Burstable Performance)

Examples:

- t2.micro
- t3.micro
- t3.small
- t4g.micro

Features:

- Low cost
- CPU Credits
- Burstable performance

Best For:

- Development
- Testing
- Small websites
- Learning AWS

Advantages:

- Cheapest option
- Free Tier eligible (selected generations)
- Good for low-traffic workloads

Limitations:

- Sustained high CPU usage can exhaust CPU credits.

---

## M Family

Balanced compute and memory.

Examples:

- m5.large
- m6i.large
- m7g.large

Best For:

- Business applications
- Medium traffic websites
- Enterprise workloads

---

# 2. Compute Optimized

Family:

- C

Examples:

- c5.large
- c6i.large
- c7g.large

Designed for:

- High CPU applications
- Gaming
- API servers
- Scientific computing
- Video encoding

Advantages:

- High CPU performance
- Lower latency

---

# 3. Memory Optimized

Families:

- R
- X
- z

Examples:

- r6i.large
- r7g.large
- x2idn

Designed for:

- Databases
- SAP
- Redis
- Elasticsearch
- In-memory analytics

Advantages:

- Large RAM
- Fast memory access

---

# 4. Storage Optimized

Families:

- I
- D
- H

Examples:

- i4i.large
- d3.large
- h1.large

Best For:

- NoSQL databases
- Big Data
- Hadoop
- Kafka
- Log processing

Advantages:

- High local storage performance

---

# 5. Accelerated Computing

Families:

- G
- P
- F
- Inf
- Trn

Examples:

- g5.xlarge
- p5.48xlarge

Best For:

- AI/ML
- Deep Learning
- Image Processing
- Video Rendering
- GPU workloads

---

# 6. HPC (High Performance Computing)

Designed for:

- Weather simulation
- Scientific research
- Financial modeling
- Engineering simulations

Features:

- High-speed networking
- Low latency
- Large compute capacity

---

# ARM vs x86

AWS supports different processor architectures.

## x86

Processors:

- Intel Xeon
- AMD EPYC

Best For:

- Legacy applications
- Commercial software

Examples:

- m6i
- c6i
- r6i

---

## ARM (AWS Graviton)

Processor:

AWS Graviton

Examples:

- t4g
- m7g
- c7g
- r7g

Advantages:

- Better price-performance
- Lower power consumption
- Improved efficiency

Choose Graviton only if your applications support ARM architecture.

---

# Nitro System

Most modern EC2 instances run on the AWS Nitro System.

Benefits:

- Better security
- Better performance
- Hardware virtualization
- Dedicated networking
- Dedicated storage processing

---

# Instance Sizes

Example:

| Size | Relative Capacity |
|------|-------------------|
| nano | Smallest |
| micro | Very Small |
| small | Small |
| medium | Medium |
| large | Standard |
| xlarge | Large |
| 2xlarge | Double Large |
| 4xlarge | Larger |
| 8xlarge | High Capacity |
| 16xlarge | Enterprise |
| metal | Bare Metal |

---

# Choosing the Right Instance

| Workload | Recommended Family |
|----------|--------------------|
| Personal Website | t3.micro |
| Apache/Nginx | t3.small |
| Tomcat | t3.medium |
| Jenkins | t3.large |
| Production Web App | m6i.large |
| Java Application | m6i.large |
| MySQL | r6i.large |
| PostgreSQL | r6i.large |
| Redis | r7g.large |
| Hadoop | i4i.large |
| Kafka | i4i.large |
| AI Training | p5 |
| Machine Learning Inference | inf2 |
| Gaming | c6i.large |

---

# Real Production Example

A banking application:

```
Internet

↓

Application Load Balancer

↓

Web Layer

↓

M Family

↓

Application Layer

↓

C Family

↓

Database Layer

↓

R Family

↓

Analytics

↓

I Family
```

Each layer uses a different instance family based on workload requirements.

---

# Best Practices

- Choose the smallest instance that meets your needs.
- Monitor CPU and memory utilization with CloudWatch.
- Use Auto Scaling for changing workloads.
- Prefer newer generation instances.
- Evaluate Graviton instances for supported applications.
- Test performance before migrating production workloads.

---

# Common Mistakes

❌ Selecting oversized instances.

❌ Running databases on burstable T instances.

❌ Ignoring CPU credit limits on T family.

❌ Using GPU instances for normal web applications.

❌ Choosing old-generation instance families without a reason.

---

# Interview Questions

## Beginner

1. What is an EC2 Instance Type?
2. What does `t3.micro` mean?
3. What are EC2 instance families?
4. What is the difference between General Purpose and Compute Optimized instances?
5. What is the T family used for?

---

## Intermediate

1. Explain the naming convention of `m7g.large`.
2. Compare T, M, C, and R families.
3. What is AWS Graviton?
4. What is the Nitro System?
5. Which instance type would you choose for a production database and why?

---

# Quick Revision

```
General Purpose

↓

T, M

↓

Compute

↓

C

↓

Memory

↓

R, X

↓

Storage

↓

I, D, H

↓

Accelerated

↓

G, P, F, Inf, Trn

↓

HPC
```

---

# Summary

Amazon EC2 offers a wide range of instance types to support different workloads. Choosing the correct family and size helps optimize performance and cost. General Purpose instances suit most applications, Compute Optimized instances handle CPU-intensive workloads, Memory Optimized instances are ideal for databases, Storage Optimized instances support high I/O applications, and Accelerated Computing instances power AI and GPU-based tasks.

---

# Related Topics

- EC2 Pricing
- Auto Scaling
- Elastic Load Balancer
- Amazon EBS
- AWS Nitro System
- AWS Graviton

---

# References

- AWS EC2 Instance Types Documentation: https://docs.aws.amazon.com/ec2/latest/instancetypes/
- AWS EC2 Pricing: https://aws.amazon.com/ec2/pricing/