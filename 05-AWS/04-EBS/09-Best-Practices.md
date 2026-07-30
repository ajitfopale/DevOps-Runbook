# Amazon EBS Best Practices

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Intermediate to Advanced  
> **Estimated Reading Time:** 35 Minutes

---

# Objective

After reading this document, you will understand:

- AWS recommended EBS best practices
- Performance optimization
- High Availability
- Security recommendations
- Backup and Disaster Recovery
- Cost optimization
- Monitoring and automation
- Production deployment checklist

---

# Introduction

Amazon EBS is designed to provide durable, persistent, and high-performance block storage for Amazon EC2.

Following AWS best practices helps organizations achieve:

- High Availability
- Better Performance
- Improved Security
- Lower Costs
- Faster Recovery
- Easier Operations

---

# EBS Best Practice Areas

```
Amazon EBS

│

├── Volume Selection

├── Performance

├── Security

├── Backup

├── Monitoring

├── Automation

├── Cost Optimization

└── Disaster Recovery
```

---

# 1. Choose the Correct Volume Type

Do not use the same volume type for every workload.

| Workload | Recommended Volume |
|----------|--------------------|
| Boot Volume | gp3 |
| Web Server | gp3 |
| Application Server | gp3 |
| Database | io2 / gp3 |
| Analytics | io2 |
| Backup | st1 / sc1 |

Choosing the right volume type improves both performance and cost efficiency.

---

# 2. Prefer gp3 Over gp2

AWS recommends gp3 for most workloads.

Advantages

- Lower price
- Better baseline performance
- Independent IOPS
- Independent Throughput

Example

```
Old

gp2

↓

New

gp3
```

---

# 3. Encrypt All Production Volumes

Enable encryption during volume creation.

Benefits

- Protects sensitive data
- Meets compliance requirements
- Works with AWS KMS

Encrypt

- Root Volume
- Data Volume
- Snapshots

---

# 4. Use IAM for Access Control

Grant only required permissions.

Avoid

```
AdministratorAccess
```

Use

```
Least Privilege
```

Example

Developers

↓

Read Only

Operations Team

↓

Volume Management

---

# 5. Tag Every Resource

Recommended Tags

| Key | Value |
|------|-------|
| Name | DatabaseVolume |
| Environment | Production |
| Owner | DevOps |
| Project | Banking |
| Backup | Daily |

Benefits

- Cost Allocation
- Automation
- Resource Tracking

---

# 6. Take Regular Snapshots

Recommended Schedule

```
Daily

↓

Weekly

↓

Monthly
```

Automate using

- AWS Backup
- Amazon Data Lifecycle Manager

Always test snapshot restoration.

---

# 7. Monitor Performance

Use Amazon CloudWatch.

Monitor

- Volume Read Ops
- Volume Write Ops
- Read Latency
- Write Latency
- Queue Length
- Burst Balance (gp2)
- Throughput

Create alarms for abnormal behavior.

---

# 8. Resize Before Running Out of Space

Do not wait until the disk is full.

Monitor storage usage.

Increase the volume size before it reaches critical levels.

Example

```
80%

↓

Increase Volume

↓

Extend Filesystem
```

---

# 9. Use UUID in /etc/fstab

Avoid using device names.

Incorrect

```
/dev/nvme1n1p1
```

Correct

```
UUID=xxxxxxxx
```

Benefits

- Reliable mounting
- Consistent across reboots

Verify configuration

```bash
sudo mount -a
```

---

# 10. Delete Unused Resources

Regularly review

- Unattached Volumes
- Old Snapshots
- Unused AMIs

Unused resources increase AWS costs.

---

# 11. Separate OS and Application Data

Recommended

```
EC2

↓

Root Volume

↓

Operating System

+

Data Volume

↓

Application Data
```

Benefits

- Easier backup
- Independent scaling
- Better recovery

---

# 12. Enable Fast Snapshot Restore Only When Required

Fast Snapshot Restore (FSR)

Advantages

- Immediate performance after restore

Disadvantage

- Additional hourly charges

Use only for critical workloads.

---

# 13. Test Disaster Recovery

A backup is useful only if it can be restored.

Regularly perform

- Snapshot restore
- Volume creation
- Application validation

Document recovery procedures.

---

# 14. Automate Storage Management

Automate

- Snapshots
- Cleanup
- Tagging
- Monitoring
- Notifications

Tools

- AWS Backup
- Data Lifecycle Manager
- Lambda
- EventBridge

---

# 15. Plan for High Availability

Remember

An EBS volume belongs to one Availability Zone.

For Disaster Recovery

```
EBS Snapshot

↓

Copy Snapshot

↓

Another Region

↓

Restore Volume
```

---

# Production Architecture

```
Application

↓

EC2

↓

Encrypted gp3 Volume

↓

CloudWatch

↓

Daily Snapshot

↓

AWS Backup

↓

Cross Region Copy

↓

Disaster Recovery
```

---

# Performance Best Practices

- Select the correct volume type.
- Monitor IOPS.
- Monitor throughput.
- Use io2 for mission-critical databases.
- Separate log and data disks for databases.
- Resize volumes proactively.

---

# Cost Optimization Best Practices

- Use gp3 where possible.
- Delete unattached volumes.
- Remove obsolete snapshots.
- Automate snapshot retention.
- Monitor AWS Cost Explorer.
- Avoid overprovisioning storage.

---

# Operational Best Practices

- Document mount points.
- Keep `/etc/fstab` updated.
- Label volumes consistently.
- Test recovery procedures.
- Review storage usage monthly.
- Audit backup jobs regularly.

---

# Production Deployment Checklist

Before deploying:

- Correct volume type selected
- Encryption enabled
- IAM permissions verified
- Tags applied
- Snapshot schedule configured
- CloudWatch monitoring enabled
- Backup tested
- `/etc/fstab` validated
- Recovery procedure documented

---

# Common Mistakes

❌ Using gp2 for all workloads

❌ Not encrypting production volumes

❌ Forgetting snapshots

❌ Never testing restores

❌ Leaving unattached volumes

❌ Hardcoding device names in `/etc/fstab`

❌ Ignoring CloudWatch alarms

❌ Running out of disk space

---

# Interview Questions

## Beginner

1. Why is gp3 recommended over gp2?
2. Why should EBS volumes be encrypted?
3. Why should snapshots be automated?
4. What is the purpose of tagging?
5. Why should UUID be used in `/etc/fstab`?

---

## Intermediate

1. Explain EBS performance monitoring.
2. How do you optimize EBS costs?
3. What is Fast Snapshot Restore?
4. How would you design a backup strategy for EBS?
5. Explain best practices for production EBS deployments.

---

# Quick Revision

```
Correct Volume Type

↓

gp3

↓

Encryption

↓

IAM

↓

Tags

↓

Snapshots

↓

CloudWatch

↓

Resize

↓

UUID

↓

Automation

↓

Disaster Recovery
```

---

# Summary

Amazon EBS best practices focus on selecting the right volume type, encrypting storage, monitoring performance, automating backups, optimizing costs, and regularly testing disaster recovery procedures. By following these recommendations, organizations can build secure, scalable, and highly available storage solutions for production workloads.

---

# Related Topics

- Amazon EC2
- Amazon EBS Snapshots
- AWS Backup
- Amazon CloudWatch
- AWS KMS
- AWS Data Lifecycle Manager

---

# References

- Amazon EBS User Guide
- AWS Storage Best Practices
- AWS Well-Architected Framework
- AWS Backup Documentation