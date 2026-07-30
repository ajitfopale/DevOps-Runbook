# Amazon EBS Cheat Sheet

> **Quick Revision Guide for AWS Interviews & Production**
>
> **Category:** AWS Storage
>
> **Service:** Amazon Elastic Block Store (EBS)

---

# What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a **persistent block storage service** for Amazon EC2.

- Persistent Storage
- Low Latency
- High Performance
- Block Storage
- Supports Snapshots
- Supports Encryption

---

# Key Features

- Persistent storage
- Block-level storage
- SSD & HDD volume types
- Snapshots
- Encryption using AWS KMS
- Resize without downtime
- Multi-Attach (io1/io2)
- CloudWatch Monitoring

---

# EBS Lifecycle

```
Create Volume

↓

Attach to EC2

↓

Format

↓

Mount

↓

Read / Write Data

↓

Snapshot

↓

Modify Volume

↓

Extend Filesystem

↓

Detach

↓

Delete
```

---

# Volume Types

| Volume | Best For |
|---------|----------|
| gp3 | General workloads |
| gp2 | Legacy SSD |
| io2 | Mission-critical databases |
| io1 | Legacy provisioned IOPS |
| st1 | Throughput-intensive HDD |
| sc1 | Cold storage |

---

# Recommended Volume Types

| Workload | Volume |
|----------|--------|
| Linux Boot | gp3 |
| Windows Boot | gp3 |
| Web Server | gp3 |
| Application Server | gp3 |
| MySQL | gp3 / io2 |
| PostgreSQL | gp3 / io2 |
| Oracle | io2 |
| SQL Server | io2 |
| SAP | io2 |
| Jenkins | gp3 |
| Kubernetes | gp3 |

---

# Important Linux Commands

## List disks

```bash
lsblk
```

---

## Show mounted filesystems

```bash
df -h
```

---

## Show UUID

```bash
blkid
```

---

## Show partitions

```bash
fdisk -l
```

---

## Show filesystem type

```bash
file -s /dev/nvme1n1
```

---

## Create filesystem

XFS

```bash
mkfs.xfs /dev/nvme1n1p1
```

EXT4

```bash
mkfs.ext4 /dev/nvme1n1p1
```

---

## Mount volume

```bash
mount /dev/nvme1n1p1 /data
```

---

## Unmount

```bash
umount /data
```

---

## Grow XFS

```bash
xfs_growfs /data
```

---

## Grow EXT4

```bash
resize2fs /dev/nvme1n1p1
```

---

# Important AWS CLI Commands

## List Volumes

```bash
aws ec2 describe-volumes
```

---

## Create Volume

```bash
aws ec2 create-volume \
--availability-zone ap-south-1a \
--size 20 \
--volume-type gp3
```

---

## Attach Volume

```bash
aws ec2 attach-volume \
--volume-id vol-xxxx \
--instance-id i-xxxx \
--device /dev/xvdf
```

---

## Detach Volume

```bash
aws ec2 detach-volume \
--volume-id vol-xxxx
```

---

## Modify Volume

```bash
aws ec2 modify-volume \
--volume-id vol-xxxx \
--size 50
```

---

## Create Snapshot

```bash
aws ec2 create-snapshot \
--volume-id vol-xxxx
```

---

## Delete Snapshot

```bash
aws ec2 delete-snapshot \
--snapshot-id snap-xxxx
```

---

## Delete Volume

```bash
aws ec2 delete-volume \
--volume-id vol-xxxx
```

---

# Snapshot Facts

- Point-in-time backup
- First snapshot is full
- Subsequent snapshots are incremental
- Stored by AWS
- Can restore volumes
- Can copy across Regions
- Can be encrypted

---

# Security Checklist

✅ Enable encryption

✅ Use AWS KMS

✅ Apply IAM Least Privilege

✅ Enable CloudTrail

✅ Enable CloudWatch

✅ Encrypt snapshots

✅ Tag resources

---

# Monitoring Metrics

CloudWatch Metrics

- VolumeReadOps
- VolumeWriteOps
- VolumeReadBytes
- VolumeWriteBytes
- VolumeQueueLength
- ReadLatency
- WriteLatency
- BurstBalance (gp2)

---

# Best Practices

- Use gp3 for most workloads.
- Encrypt all production volumes.
- Tag every resource.
- Automate snapshots.
- Monitor CloudWatch metrics.
- Resize before the disk is full.
- Use UUID in `/etc/fstab`.
- Test snapshot restores.
- Delete unused volumes.
- Remove obsolete snapshots.

---

# Common Errors

## Wrong Filesystem

```
wrong fs type

bad superblock
```

Fix

- Verify partition
- Verify filesystem
- Run `fsck` or `xfs_repair`

---

## Mount Failed

Check

```bash
lsblk
```

```bash
blkid
```

```bash
cat /etc/fstab
```

---

## Boot Failure

Usually caused by

```
Wrong UUID

↓

/etc/fstab
```

---

## Resize Not Visible

Run

```bash
xfs_growfs
```

or

```bash
resize2fs
```

---

# Interview One-Liners

**What is EBS?**

Persistent block storage for EC2.

---

**Can EBS survive reboot?**

Yes.

---

**Can EBS survive stop/start?**

Yes.

---

**Can EBS survive termination?**

Only if **Delete on Termination** is disabled (or for non-root volumes unless deleted manually).

---

**Can EBS attach across AZs?**

No.

---

**Best volume type?**

gp3.

---

**Best database volume?**

io2.

---

**Snapshot type?**

Incremental after the first full snapshot.

---

**Encryption service?**

AWS KMS.

---

**Monitoring service?**

CloudWatch.

---

**Audit service?**

CloudTrail.

---

# Production Workflow

```
Create Volume

↓

Attach

↓

Format

↓

Mount

↓

Application

↓

Snapshot

↓

CloudWatch

↓

AWS Backup

↓

Disaster Recovery
```

---

# EBS vs EFS vs Instance Store

| Feature | EBS | EFS | Instance Store |
|--------|-----|-----|----------------|
| Storage Type | Block | File | Local Block |
| Persistence | Yes | Yes | No |
| Shared | No (except Multi-Attach) | Yes | No |
| Availability | Single AZ | Multi-AZ | Instance Only |
| Best For | EC2 | Shared Storage | Temporary Data |

---

# Quick Memory Tricks

```
gp3

↓

General Purpose

↓

Most Workloads
```

```
io2

↓

High IOPS

↓

Databases
```

```
Snapshot

↓

Backup

↓

Restore

↓

Disaster Recovery
```

```
CloudWatch

↓

Monitor
```

```
CloudTrail

↓

Audit
```

```
KMS

↓

Encryption
```

---

# Final Revision

```
EBS

↓

Persistent Block Storage

↓

gp3

↓

Attach

↓

Format

↓

Mount

↓

Snapshot

↓

Resize

↓

CloudWatch

↓

CloudTrail

↓

KMS

↓

Production
```

---

# Summary

Amazon EBS is the primary persistent block storage solution for EC2. Mastering volume types, Linux storage management, snapshots, encryption, monitoring, troubleshooting, and AWS CLI operations is essential for AWS, DevOps, Cloud, and System Administration roles.