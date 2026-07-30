# Amazon EBS Interview Questions & Answers

> **Document Version:** 1.0
> **Category:** AWS Storage
> **Service:** Amazon Elastic Block Store (EBS)
> **Difficulty:** Beginner to Expert
> **Estimated Reading Time:** 60 Minutes

---

# Objective

This document contains frequently asked Amazon EBS interview questions with concise answers, covering:

- Fundamentals
- Architecture
- Volume Types
- Snapshots
- Security
- Performance
- Troubleshooting
- Production Scenarios

---

# Beginner Level

## 1. What is Amazon EBS?

**Answer:**

Amazon Elastic Block Store (EBS) is a block storage service that provides persistent storage for Amazon EC2 instances. It is commonly used for operating systems, databases, and applications.

---

## 2. What is block storage?

**Answer:**

Block storage stores data in fixed-size blocks. The operating system formats these blocks into a filesystem (such as XFS or ext4), allowing applications to read and write files.

---

## 3. Is EBS persistent?

**Answer:**

Yes. Data remains even if the EC2 instance is stopped or restarted. It is deleted only if the volume is deleted (or if the root volume is configured to delete on termination).

---

## 4. Can an EBS volume be attached to multiple EC2 instances?

**Answer:**

Normally, no. However, **Multi-Attach** allows compatible **io1/io2** volumes to be attached to multiple EC2 instances within the **same Availability Zone**, provided the application supports shared access.

---

## 5. Can an EBS volume be attached across Availability Zones?

**Answer:**

No. An EBS volume and the EC2 instance must be in the same Availability Zone.

---

## 6. Which AWS service creates EBS snapshots?

**Answer:**

Amazon EBS itself creates snapshots, which are stored in Amazon S3 (AWS manages the underlying storage).

---

## 7. What happens when an EC2 instance stops?

**Answer:**

The EBS volume remains intact and the data persists.

---

## 8. Which volume type should be used for most workloads?

**Answer:**

AWS recommends **gp3** because it offers better price-performance than gp2.

---

## 9. What command displays attached disks in Linux?

```bash
lsblk
```

---

## 10. What command displays mounted filesystems?

```bash
df -h
```

---

# Intermediate Level

## 11. What is the difference between gp3 and gp2?

| gp2 | gp3 |
|------|------|
| Performance linked to size | Performance independent of size |
| Older generation | Latest generation |
| Higher cost for similar performance | Better price-performance |

---

## 12. What is an EBS Snapshot?

**Answer:**

A point-in-time backup of an EBS volume that can be used to restore data or create new volumes.

---

## 13. Are snapshots full backups?

**Answer:**

No. The first snapshot is full, while subsequent snapshots are incremental, storing only changed blocks.

---

## 14. Can you resize an EBS volume?

**Answer:**

Yes. Increase the volume size using AWS, then extend the filesystem inside Linux.

---

## 15. How do you extend an XFS filesystem?

```bash
sudo xfs_growfs /mount-point
```

---

## 16. How do you extend an ext4 filesystem?

```bash
sudo resize2fs /dev/device
```

---

## 17. Why use UUID in `/etc/fstab`?

**Answer:**

UUID remains consistent across reboots, while device names (such as `/dev/nvme1n1`) may change.

---

## 18. Which AWS service manages EBS encryption keys?

**Answer:**

AWS Key Management Service (AWS KMS).

---

## 19. Which service monitors EBS metrics?

**Answer:**

Amazon CloudWatch.

---

## 20. Which service records EBS API activity?

**Answer:**

AWS CloudTrail.

---

# Advanced Level

## 21. Explain the EBS lifecycle.

**Answer:**

Create Volume → Attach → Format → Mount → Read/Write Data → Snapshot → Resize (if needed) → Detach/Delete.

---

## 22. How do you migrate a volume to another Region?

**Answer:**

Create a snapshot → Copy the snapshot to the destination Region → Create a new volume from the copied snapshot.

---

## 23. How would you reduce EBS costs?

**Answer:**

- Use gp3 instead of gp2.
- Delete unattached volumes.
- Remove obsolete snapshots.
- Automate retention with Data Lifecycle Manager.
- Monitor usage with AWS Cost Explorer.

---

## 24. What is Fast Snapshot Restore (FSR)?

**Answer:**

A feature that allows volumes restored from snapshots to achieve full performance immediately, at an additional cost.

---

## 25. What are the common CloudWatch metrics for EBS?

- VolumeReadOps
- VolumeWriteOps
- VolumeReadBytes
- VolumeWriteBytes
- VolumeQueueLength
- VolumeIdleTime
- ReadLatency
- WriteLatency

---

# Scenario-Based Questions

## 26. An attached EBS volume is not visible in Linux. What do you check?

**Answer:**

1. Verify the volume is attached in AWS.
2. Confirm the instance and volume are in the same AZ.
3. Run `lsblk`.
4. Run `dmesg | grep nvme`.
5. Check whether a partition or filesystem exists.

---

## 27. After increasing the volume size, Linux still shows the old capacity. Why?

**Answer:**

The filesystem has not been extended. Run:

- XFS:

```bash
sudo xfs_growfs /mount-point
```

- ext4:

```bash
sudo resize2fs /dev/device
```

---

## 28. Why does `/etc/fstab` cause boot failures?

**Answer:**

An incorrect UUID or mount configuration can prevent Linux from mounting filesystems during boot, causing the instance to enter emergency mode.

---

## 29. What would you do before detaching a volume?

**Answer:**

- Stop applications using the disk.
- Unmount the filesystem.
- Verify no processes are using the mount point (`lsof`).
- Detach the volume from AWS.

---

## 30. How do you investigate high EBS latency?

**Answer:**

- Review CloudWatch metrics.
- Check `iostat` and `vmstat`.
- Verify IOPS and throughput requirements.
- Upgrade to gp3 or io2 if needed.
- Identify applications generating heavy I/O.

---

# Practical Linux Questions

## Display block devices

```bash
lsblk
```

## Show filesystem usage

```bash
df -h
```

## Display UUID

```bash
blkid
```

## List mounted filesystems

```bash
mount
```

## Show partitions

```bash
fdisk -l
```

## Check filesystem type

```bash
file -s /dev/nvme1n1
```

---

# Rapid Fire Questions

1. What does EBS stand for?
2. Is EBS persistent?
3. Can EBS be shared across AZs?
4. Which volume type is recommended for most workloads?
5. Which volume type is best for databases?
6. What is Multi-Attach?
7. Which service encrypts EBS?
8. Which service monitors EBS?
9. Which service audits EBS API calls?
10. What command lists block devices?
11. What command displays mounted filesystems?
12. What is a snapshot?
13. Are snapshots incremental?
14. Can EBS volumes be resized?
15. Can EBS volumes be encrypted after creation? *(By creating an encrypted copy/snapshot and restoring, or using the EBS encryption workflow.)*

---

# Interview Tips

- Understand the difference between gp3 and io2.
- Be comfortable with Linux storage commands.
- Explain snapshots clearly.
- Know how to resize both the volume and the filesystem.
- Understand encryption using AWS KMS.
- Mention CloudWatch and CloudTrail when discussing monitoring and auditing.
- Practice real troubleshooting scenarios.

---

# Summary

Amazon EBS interview questions typically focus on storage fundamentals, volume types, snapshots, encryption, Linux administration, monitoring, troubleshooting, and production best practices. A strong understanding of both AWS concepts and Linux storage management is essential for AWS, DevOps, Cloud, and System Administrator roles.

---

# References

- Amazon EBS User Guide
- AWS Storage Blog
- AWS Well-Architected Framework