# Amazon EBS (Elastic Block Store) - Introduction

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Beginner  
> **Estimated Reading Time:** 20 Minutes

---

# Objective

After reading this document, you will understand:

- What Amazon EBS is
- Why EBS is required
- How EBS works
- Features of EBS
- Where EBS is used
- Advantages and limitations
- Real-world production examples
- Common interview questions

---

# What is Amazon EBS?

Amazon **Elastic Block Store (EBS)** is a **block storage service** provided by AWS.

It provides **persistent storage** for Amazon EC2 instances.

Think of EBS as the **hard disk (SSD/HDD)** attached to a virtual machine.

If an EC2 instance is the **computer**, then EBS is its **hard drive**.

---

# Official Definition

Amazon Elastic Block Store (Amazon EBS) provides persistent block-level storage volumes for use with Amazon EC2 instances.

---

# Simple Definition

Amazon EBS is a virtual hard disk that stores the operating system, applications, and data for an EC2 instance.

Unlike temporary instance storage, EBS keeps your data even after the EC2 instance is stopped.

---

# Why Do We Need EBS?

Imagine you launch an EC2 instance.

Without storage, the server cannot:

- Install an operating system
- Store applications
- Save databases
- Store logs
- Keep user files

EBS solves this problem by providing reliable storage.

---

# Real-Life Example

Think about your laptop.

Your laptop has:

- CPU
- RAM
- Hard Disk (SSD)

Similarly, an EC2 instance has:

- vCPU
- Memory (RAM)
- EBS Volume (Virtual Hard Disk)

Without a hard disk, your laptop cannot store files.

Without EBS, an EC2 instance cannot permanently store data.

---

# Why is it called "Elastic"?

Elastic means **flexible**.

You can:

- Increase storage size
- Change volume type
- Take snapshots
- Attach additional volumes
- Resize without rebuilding the server

---

# Key Features

- Persistent storage
- High availability
- High durability
- SSD and HDD options
- Encryption support
- Snapshots
- Easy resizing
- Fast performance
- Integration with EC2
- Backup using snapshots

---

# EBS Characteristics

| Feature | Description |
|---------|-------------|
| Storage Type | Block Storage |
| Persistent | Yes |
| Encryption | Supported |
| Snapshot Support | Yes |
| Attach to EC2 | Yes |
| Availability Zone Specific | Yes |
| Boot Volume | Supported |

---

# Where is EBS Used?

Amazon EBS is commonly used for:

- Operating System storage
- Application installation
- Database storage
- Log storage
- Web servers
- ERP applications
- Banking applications
- Enterprise applications
- File storage
- Backup volumes

---

# How EBS Works (High Level)

```
          User

            │

            ▼

      Launch EC2

            │

            ▼

     AWS Creates EBS

            │

            ▼

     EBS Attached to EC2

            │

            ▼

   Operating System Boots

            │

            ▼

     Data Stored on EBS
```

---

# Types of Data Stored on EBS

Examples include:

- Linux operating system
- Windows operating system
- Apache web server
- Nginx
- MySQL database
- Application files
- User uploads
- Log files
- Configuration files

---

# Persistence Example

Suppose you create an EC2 instance.

You install:

- Apache
- Java
- MySQL

You create:

```
/home/ec2-user/project
```

Now stop the instance.

After starting it again:

- Apache is still installed.
- Java is still available.
- Your project files remain.

This happens because the data is stored on EBS.

---

# Real Production Example

A company hosts an online banking application.

The EC2 instance runs:

- Java application
- Apache Tomcat

The EBS volume stores:

- Operating System
- Application binaries
- Configuration files
- Logs

Every night, AWS creates EBS snapshots.

If the server fails, a new EBS volume can be created from the snapshot and attached to another EC2 instance, reducing recovery time.

---

# Advantages

- Persistent storage
- High performance
- Reliable
- Easy backup
- Easy restore
- Resize anytime
- Encryption available
- Snapshot support
- Cost effective
- Fully managed by AWS

---

# Limitations

- Can only be attached within the same Availability Zone.
- Not designed to be shared by multiple EC2 instances (except specific Multi-Attach scenarios).
- Additional cost for snapshots and provisioned storage.
- Performance depends on the selected volume type.

---

# Important Terms

| Term | Meaning |
|------|---------|
| EBS | Elastic Block Store |
| Volume | Virtual disk |
| Snapshot | Backup of an EBS volume |
| AZ | Availability Zone |
| gp3 | General Purpose SSD |
| io2 | High-performance SSD |
| st1 | Throughput Optimized HDD |
| sc1 | Cold HDD |

---

# Best Practices

- Use gp3 for most workloads.
- Encrypt production volumes.
- Schedule regular snapshots.
- Monitor disk usage with CloudWatch.
- Delete unused volumes.
- Tag all EBS volumes.
- Use separate volumes for databases and logs when appropriate.

---

# Common Beginner Mistakes

- Deleting an EBS volume accidentally.
- Forgetting to create snapshots before major changes.
- Launching a volume in a different Availability Zone than the EC2 instance.
- Choosing the wrong volume type.
- Leaving unattached volumes running and paying for unused storage.

---

# Interview Questions

## Beginner

1. What is Amazon EBS?
2. What type of storage is EBS?
3. Why is EBS called Elastic?
4. Can EBS be attached to EC2?
5. Is EBS persistent storage?

## Intermediate

1. What is the difference between EBS and Instance Store?
2. Why are snapshots important?
3. Can an EBS volume be attached to multiple instances?
4. Why must an EBS volume and EC2 instance be in the same Availability Zone?
5. Which EBS volume type would you choose for a database?

---

# Summary

Amazon EBS is the primary persistent block storage service for Amazon EC2. It stores operating systems, applications, databases, logs, and user data while offering high availability, snapshots, encryption, and flexible resizing. Understanding EBS is essential because almost every EC2-based workload relies on it for durable storage.

---

# Related Topics

- EC2
- AMI
- Snapshots
- Volume Types
- EFS
- S3
- CloudWatch

---

# References

- AWS Official Documentation: https://docs.aws.amazon.com/ebs/
- AWS EBS User Guide: https://docs.aws.amazon.com/ebs/latest/userguide/