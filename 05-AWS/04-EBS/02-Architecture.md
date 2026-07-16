# Amazon EBS Architecture

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 20–25 Minutes

---

# Objective

After reading this document, you will understand:

- EBS architecture
- How EBS communicates with EC2
- Internal working of EBS
- Data flow between EC2 and EBS
- Availability Zone relationship
- Data replication
- Real production architecture

---

# What is EBS Architecture?

Amazon EBS architecture explains how AWS provides reliable, high-performance block storage to EC2 instances.

Although EBS appears like a local hard disk inside the operating system, it is actually a **network-attached storage service** managed by AWS.

The operating system treats it like a normal disk, but AWS stores and protects the data in its storage infrastructure.

---

# High-Level Architecture

```text
                  AWS Region
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
   Availability Zone A         Availability Zone B
        │
        │
        ▼
+-----------------------------+
|         EC2 Instance        |
|                             |
|   Linux / Windows OS        |
|   Applications              |
|   Database                  |
+-------------+---------------+
              │
              │ Block Storage
              ▼
+-----------------------------+
|       Amazon EBS Volume     |
|                             |
|   gp3 / io2 / st1 / sc1     |
+-------------+---------------+
              │
              ▼
 AWS Storage Infrastructure
 (Replicated within the AZ)
```

---

# Main Components

## 1. EC2 Instance

The EC2 instance is the compute resource.

It runs:

- Operating System
- Applications
- Databases
- Web Servers

The EC2 instance does **not** permanently store data by itself.

Instead, it reads from and writes to the attached EBS volume.

---

## 2. EBS Volume

The EBS volume acts like a virtual hard disk.

It stores:

- Operating System
- User files
- Databases
- Logs
- Applications

Every volume has:

- Volume ID
- Size
- Volume Type
- Availability Zone

Example:

```
Volume ID

vol-0123456789abcdef
```

---

## 3. Availability Zone

Every EBS volume belongs to **one Availability Zone (AZ).**

Example:

```
Mumbai Region

↓

ap-south-1a
```

An EC2 instance can attach an EBS volume **only if both are in the same Availability Zone.**

---

## 4. AWS Storage Infrastructure

Behind the scenes, AWS stores EBS data on highly available storage systems.

AWS automatically replicates the data **within the same Availability Zone** to protect against hardware failures.

This replication is handled by AWS and does not require user configuration.

---

## 5. Hypervisor

AWS uses a virtualization layer (hypervisor) to connect the EC2 instance to the EBS volume.

Responsibilities include:

- Attaching volumes
- Processing read/write requests
- Isolating virtual machines
- Managing storage access

Users do not interact with the hypervisor directly.

---

# Data Flow

When an application writes data:

```text
Application

↓

Operating System

↓

File System

↓

EBS Driver

↓

Network Storage

↓

Amazon EBS

↓

AWS Storage Infrastructure
```

When reading data:

```text
AWS Storage Infrastructure

↓

Amazon EBS

↓

Operating System

↓

Application
```

---

# EBS Attachment Flow

```text
User

↓

Launch EC2

↓

Create EBS Volume

↓

Attach Volume

↓

Operating System Detects Disk

↓

Create Partition

↓

Create File System

↓

Mount Volume

↓

Ready for Use
```

---

# Architecture Example

```text
                Internet
                    │
                    ▼
             Application Users
                    │
                    ▼
              Load Balancer
                    │
      ┌─────────────┴─────────────┐
      ▼                           ▼
+-------------+             +-------------+
| EC2 Server1 |             | EC2 Server2 |
+------+------+             +------+------+
       │                           │
       ▼                           ▼
+-------------+             +-------------+
| EBS Volume  |             | EBS Volume  |
+-------------+             +-------------+

             Database on Amazon RDS
```

Each EC2 instance has its own EBS volume.

---

# Why EBS is Reliable

AWS provides reliability by:

- Replicating data within the Availability Zone
- Monitoring storage hardware
- Replacing failed hardware automatically
- Supporting snapshots for backups
- Encrypting data when enabled

---

# Relationship Between EC2 and EBS

| EC2 | EBS |
|------|-----|
| Compute | Storage |
| Runs applications | Stores data |
| Can stop/start | Data remains |
| Uses CPU/RAM | Uses storage capacity |

---

# Real Production Example

Suppose a banking application runs on EC2.

The EBS volume stores:

- Operating System
- Banking application
- Configuration files
- Logs

Every night:

- An EBS Snapshot is created.

If the EC2 instance fails:

1. Launch a new EC2 instance.
2. Create a volume from the snapshot.
3. Attach the volume.
4. Start the application.

This minimizes downtime and helps recover data quickly.

---

# Best Practices

- Keep EC2 and EBS in the same Availability Zone.
- Use gp3 for most workloads.
- Monitor IOPS and throughput.
- Schedule snapshots.
- Encrypt production volumes.
- Tag volumes consistently.
- Delete unused volumes to reduce costs.

---

# Common Mistakes

- Creating an EBS volume in a different Availability Zone.
- Forgetting to mount a newly attached volume.
- Not updating `/etc/fstab` after mounting.
- Deleting a volume without taking a snapshot.
- Choosing an inappropriate volume type.

---

# Interview Questions

## Beginner

1. What is Amazon EBS architecture?
2. Why must an EBS volume and EC2 instance be in the same Availability Zone?
3. Does EBS behave like local storage?
4. Is EBS network-attached storage?
5. What is stored on an EBS volume?

## Intermediate

1. Explain the EBS data flow.
2. How does AWS protect EBS data from hardware failures?
3. What is the role of the hypervisor?
4. Why are snapshots important?
5. How would you recover an EC2 instance using an EBS snapshot?

---

# Summary

Amazon EBS provides persistent, block-level storage for EC2 instances. Although it appears as a local disk to the operating system, it is a managed, network-attached storage service. AWS automatically replicates EBS data within an Availability Zone, making it reliable and suitable for production workloads.

---

# Related Topics

- EC2
- AMI
- Snapshots
- Volume Types
- CloudWatch
- AWS Backup

---

# References

- AWS Official EBS Documentation: https://docs.aws.amazon.com/ebs/
- AWS EC2 User Guide: https://docs.aws.amazon.com/ec2/