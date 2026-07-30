# Amazon EBS Pricing

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Intermediate  
> **Estimated Reading Time:** 30 Minutes

---

# Objective

After reading this document, you will understand:

- How Amazon EBS pricing works
- Factors that affect EBS cost
- Volume pricing
- Snapshot pricing
- Data transfer charges
- Cost optimization techniques
- Real-world pricing examples
- Best practices for reducing storage costs

---

# Introduction

Amazon EBS is a **pay-as-you-go block storage service**.

Unlike traditional storage systems, you only pay for the resources you provision and use.

EBS pricing depends on multiple factors, including:

- Volume Type
- Provisioned Storage (GB)
- Provisioned IOPS
- Provisioned Throughput
- Snapshots
- Data Transfer (certain scenarios)

---

# EBS Pricing Components

```
Amazon EBS Pricing

│

├── Volume Storage (GB)

├── Provisioned IOPS

├── Provisioned Throughput

├── Snapshots

├── Fast Snapshot Restore

├── EBS Direct APIs

└── Data Transfer
```

---

# 1. Volume Storage Cost

The primary charge is based on the amount of storage you provision.

Example

```
100 GB gp3 Volume

↓

Monthly Storage Charge
```

Even if only 20 GB of data is stored, you pay for the full **100 GB** that was provisioned.

---

# 2. Volume Type Pricing

Different EBS volume types have different pricing.

| Volume Type | Pricing Level | Typical Use |
|--------------|--------------|-------------|
| gp3 | Low | General Purpose |
| gp2 | Medium | Legacy Workloads |
| io2 | High | Critical Databases |
| io1 | High | Provisioned IOPS |
| st1 | Low | Throughput Workloads |
| sc1 | Lowest | Cold Storage |

---

# 3. Provisioned IOPS Charges

Applicable mainly to:

- io1
- io2

Example

```
Volume

↓

20,000 IOPS

↓

Additional Monthly Cost
```

Higher IOPS means better performance but increased cost.

---

# 4. Provisioned Throughput Charges

Applicable mainly to:

- gp3

With gp3:

- Storage
- IOPS
- Throughput

can be configured independently.

Example

```
100 GB gp3

+

8000 IOPS

+

500 MB/s Throughput
```

Higher throughput results in additional charges.

---

# 5. Snapshot Pricing

Snapshots are stored in Amazon S3.

You pay only for changed blocks.

Example

```
100 GB Volume

↓

Snapshot 1

↓

100 GB Stored

↓

Modify 5 GB

↓

Snapshot 2

↓

Only 5 GB Additional Storage
```

Snapshots are **incremental**, making them cost-efficient.

---

# 6. Fast Snapshot Restore (FSR)

Fast Snapshot Restore allows new volumes to be created from snapshots with full performance immediately.

Benefits

- Faster recovery
- Reduced initialization time

Trade-off

- Additional hourly charges

Use only when required.

---

# 7. EBS Direct APIs

Amazon provides EBS Direct APIs for reading snapshot data directly.

Common Use Cases

- Backup software
- Disaster Recovery tools
- Analytics

These API calls incur additional charges.

---

# 8. Data Transfer Charges

There is generally **no additional charge** for data transferred between an EC2 instance and its attached EBS volume within the same Availability Zone.

Charges may apply for:

- Cross-Region snapshot copy
- Cross-Region data transfer

---

# Pricing Example 1

Development Server

```
30 GB gp3

↓

Low Cost

↓

No Extra IOPS
```

Suitable for:

- Testing
- Development
- Learning

---

# Pricing Example 2

Production Database

```
1 TB io2

+

20,000 IOPS

↓

Higher Monthly Cost

↓

High Performance
```

Suitable for:

- Banking
- Financial Systems
- ERP

---

# Pricing Example 3

Backup Storage

```
500 GB Snapshot

↓

Incremental Backups

↓

Lower Storage Cost
```

---

# Cost Optimization Strategies

## Use gp3 Instead of gp2

Advantages

- Lower cost
- Better performance
- Independent IOPS and throughput

---

## Delete Unused Volumes

Common mistake

```
EC2 Terminated

↓

EBS Volume Still Exists

↓

Monthly Charges Continue
```

Review unattached volumes regularly.

---

## Remove Old Snapshots

Unused snapshots continue to incur storage charges.

Implement a retention policy.

Example

- Daily: 7 days
- Weekly: 4 weeks
- Monthly: 12 months

---

## Right-Size Volumes

Avoid provisioning more storage than necessary.

Example

```
Need

100 GB

Provision

1 TB

↓

Unnecessary Cost
```

---

## Monitor Storage Usage

Use:

- Amazon CloudWatch
- AWS Cost Explorer
- AWS Budgets

---

## Use Snapshot Lifecycle Policies

Automate:

- Snapshot creation
- Retention
- Deletion

Benefits

- Lower operational effort
- Reduced storage cost

---

# Real Production Example

```
Production Application

↓

EC2

↓

gp3 Volume

↓

Daily Snapshot

↓

AWS Backup

↓

Retention Policy

↓

CloudWatch Monitoring
```

---

# Best Practices

✔ Prefer gp3 for most workloads.

✔ Delete unused volumes.

✔ Delete obsolete snapshots.

✔ Use Lifecycle Manager.

✔ Review AWS Cost Explorer monthly.

✔ Monitor provisioned IOPS.

✔ Right-size storage.

✔ Use AWS Backup for automation.

---

# Common Cost Mistakes

❌ Leaving unattached EBS volumes.

❌ Keeping unused snapshots.

❌ Overprovisioning storage.

❌ Selecting io2 for non-critical workloads.

❌ Forgetting Fast Snapshot Restore charges.

❌ Ignoring Cost Explorer reports.

---

# Interview Questions

## Beginner

1. How is Amazon EBS priced?
2. What factors affect EBS pricing?
3. Why is gp3 cheaper than io2?
4. Are snapshots free?
5. What is incremental snapshot storage?

---

## Intermediate

1. Explain gp3 pricing.
2. Why does an unattached EBS volume still incur charges?
3. How would you reduce EBS costs?
4. What is Fast Snapshot Restore?
5. How does snapshot pricing work?

---

# Quick Revision

```
Storage (GB)

↓

Volume Type

↓

IOPS

↓

Throughput

↓

Snapshots

↓

Fast Snapshot Restore

↓

Cost Optimization
```

---

# Summary

Amazon EBS pricing is based on the storage capacity, performance configuration, and backup features that you use. By choosing the appropriate volume type, cleaning up unused resources, automating snapshot management, and monitoring costs with AWS tools, organizations can build cost-effective and high-performance storage solutions.

---

# Related Topics

- Amazon EC2
- Amazon EBS Volume Types
- Amazon EBS Snapshots
- AWS Backup
- CloudWatch
- AWS Cost Explorer
- AWS Budgets

---

# References

- AWS Pricing Calculator
- Amazon EBS Pricing Documentation
- AWS Cost Optimization Best Practices