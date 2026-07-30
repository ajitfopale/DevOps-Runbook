# Amazon EBS Troubleshooting Guide

> **Document Version:** 1.0
> **Category:** AWS Storage
> **Service:** Amazon Elastic Block Store (EBS)
> **Difficulty:** Intermediate to Advanced
> **Estimated Reading Time:** 45 Minutes

---

# Objective

This guide helps you diagnose and resolve common Amazon EBS issues in production environments.

You'll learn how to troubleshoot:

- Volume attachment failures
- Mount failures
- Filesystem corruption
- Boot failures due to `/etc/fstab`
- Volume resize issues
- Snapshot restore problems
- Performance bottlenecks
- Multi-Attach issues
- Encryption and KMS issues
- AWS CLI troubleshooting

---

# Troubleshooting Methodology

Always follow this order:

```
Identify Problem

↓

Collect Logs

↓

Check AWS Console

↓

Verify Linux

↓

Verify Filesystem

↓

Apply Fix

↓

Validate

↓

Document RCA
```

---

# Useful Commands

```bash
lsblk
```

```bash
df -h
```

```bash
blkid
```

```bash
mount
```

```bash
cat /etc/fstab
```

```bash
dmesg | tail
```

```bash
journalctl -xe
```

```bash
sudo fdisk -l
```

```bash
sudo file -s /dev/nvme1n1
```

---

# Issue 1: Volume Not Visible

## Symptoms

```
lsblk

↓

Volume Missing
```

## Possible Causes

- Volume not attached
- Wrong Availability Zone
- Attachment still in progress
- NVMe device renamed

## Verify

AWS Console

↓

Volumes

↓

State = In-use

Linux

```bash
lsblk
```

```bash
sudo fdisk -l
```

```bash
dmesg | grep nvme
```

## Resolution

- Verify EC2 and EBS are in the same AZ.
- Wait for attachment to complete.
- Reattach the volume if necessary.
- Reboot only if the operating system does not detect the device.

---

# Issue 2: Wrong Filesystem Type

## Error

```
mount:

wrong fs type

bad superblock
```

## Possible Causes

- Filesystem not created
- Wrong partition
- Corrupted filesystem

## Verify

```bash
sudo file -s /dev/nvme1n1
```

```bash
lsblk
```

```bash
blkid
```

## Resolution

Create a filesystem if the disk is new.

```bash
sudo mkfs.xfs /dev/nvme1n1p1
```

or

```bash
sudo mkfs.ext4 /dev/nvme1n1p1
```

If corruption is suspected:

```bash
sudo xfs_repair /dev/nvme1n1p1
```

or

```bash
sudo fsck.ext4 /dev/nvme1n1p1
```

---

# Issue 3: Mount Failed

## Symptoms

```
mount:

special device does not exist
```

## Verify

```bash
lsblk
```

```bash
blkid
```

```bash
cat /etc/fstab
```

## Resolution

- Verify the device name.
- Confirm the partition exists.
- Prefer UUID instead of `/dev/...` in `/etc/fstab`.

---

# Issue 4: Instance Fails to Boot After Restart

## Symptoms

EC2 Status Checks fail.

Console shows emergency mode.

## Root Cause

Incorrect `/etc/fstab`.

Example

```
Wrong UUID

↓

Boot Failure
```

## Resolution

Detach the root volume.

↓

Attach it to a rescue EC2 instance.

↓

Mount the filesystem.

↓

Edit:

```bash
/etc/fstab
```

↓

Correct the UUID or remove the invalid entry.

↓

Reattach the root volume.

---

# Issue 5: Volume Resize Not Reflected

## Symptoms

AWS shows:

```
20 GB
```

Linux still shows:

```
10 GB
```

## Verify

```bash
lsblk
```

```bash
df -h
```

## Resolution

For XFS

```bash
sudo xfs_growfs /data
```

For EXT4

```bash
sudo resize2fs /dev/nvme1n1p1
```

---

# Issue 6: Snapshot Restore Missing Files

## Possible Causes

- Wrong snapshot selected
- Snapshot taken before the data was written
- Application buffers were not flushed

## Resolution

- Verify the snapshot timestamp.
- Stop database services before taking snapshots for application-consistent backups.
- Restore the correct snapshot.

---

# Issue 7: High Disk Latency

## Symptoms

- Slow application
- Database delays
- High response time

## Monitor

CloudWatch Metrics

- VolumeReadOps
- VolumeWriteOps
- VolumeQueueLength
- ReadLatency
- WriteLatency

Linux

```bash
iostat -dx 1
```

```bash
vmstat 1
```

## Resolution

- Upgrade from gp2 to gp3.
- Increase IOPS if required.
- Separate data and log volumes.
- Remove unnecessary disk activity.

---

# Issue 8: Volume Stuck in Attaching

## Possible Causes

- EC2 instance stopped
- AWS internal delay
- Incorrect device mapping

## Verify

```bash
aws ec2 describe-volumes
```

## Resolution

- Wait a few minutes.
- Retry the attachment.
- Confirm the instance is running.
- Verify the volume and instance are in the same AZ.

---

# Issue 9: Volume Stuck in Detaching

## Possible Causes

- Filesystem still mounted
- Active I/O
- Busy application

## Resolution

Unmount first.

```bash
sudo umount /data
```

Stop applications using the disk.

Check open files.

```bash
lsof | grep /data
```

Retry detaching.

---

# Issue 10: Permission Denied

## Symptoms

```
AccessDenied
```

## Root Cause

IAM policy missing permissions.

Required permissions may include:

- ec2:CreateVolume
- ec2:AttachVolume
- ec2:DetachVolume
- ec2:CreateSnapshot
- ec2:DeleteSnapshot

Verify IAM policies and role permissions.

---

# Issue 11: Encrypted Volume Cannot Be Attached

## Possible Causes

- Missing KMS permissions
- Key disabled
- Key scheduled for deletion

## Verify

- KMS key status
- IAM role permissions
- EC2 role access

## Resolution

Grant the instance or user access to the KMS key and ensure the key is enabled.

---

# Issue 12: Snapshot Creation Slow

This is normal for large volumes.

Verify progress.

```bash
aws ec2 describe-snapshots
```

Do not delete the source volume until the snapshot reaches the **completed** state.

---

# Troubleshooting Decision Tree

```
Volume Missing?

│

├── Check AWS Attachment

│

├── Check AZ

│

├── lsblk

│

└── dmesg

↓

Mount Failed?

↓

blkid

↓

Filesystem

↓

/etc/fstab

↓

Repair

↓

Validate
```

---

# Production Troubleshooting Checklist

- Confirm EC2 instance is running.
- Verify EBS volume state.
- Check Availability Zone.
- Verify Linux detects the device.
- Confirm filesystem exists.
- Check mount point.
- Validate `/etc/fstab`.
- Review CloudWatch metrics.
- Review CloudTrail events.
- Test application functionality after recovery.

---

# Root Cause Analysis (RCA) Template

| Field | Example |
|--------|---------|
| Incident | EBS volume not mounting |
| Impact | Application unavailable |
| Root Cause | Incorrect UUID in `/etc/fstab` |
| Resolution | Updated UUID and remounted volume |
| Prevention | Use UUID verification and test with `mount -a` |

---

# Common Mistakes

❌ Attaching an EBS volume to an instance in a different Availability Zone.

❌ Formatting the wrong disk.

❌ Using device names instead of UUIDs in `/etc/fstab`.

❌ Forgetting to extend the filesystem after increasing volume size.

❌ Force-detaching active volumes.

❌ Ignoring CloudWatch alarms.

---

# Interview Questions

## Beginner

1. Why is an attached EBS volume not visible in Linux?
2. What causes the `wrong fs type` error?
3. How do you verify an EBS volume is attached?
4. Why should you use UUID in `/etc/fstab`?
5. How do you check mounted filesystems?

---

## Intermediate

1. Explain how to troubleshoot a volume resize issue.
2. How do you recover from an incorrect `/etc/fstab` entry?
3. What commands help diagnose EBS mount issues?
4. How do you investigate high EBS latency?
5. What causes a volume to remain in the `attaching` state?

---

## Advanced

1. Describe the complete troubleshooting workflow for an application outage caused by EBS.
2. How would you recover an encrypted EBS volume when KMS permissions are missing?
3. Explain how to perform RCA for a storage incident.
4. How would you troubleshoot snapshot restore inconsistencies?
5. What monitoring strategy would you implement for EBS in production?

---

# Quick Revision

```
Volume Missing
↓

lsblk

↓

blkid

↓

mount

↓

/etc/fstab

↓

CloudWatch

↓

CloudTrail

↓

Fix

↓

Validate

↓

RCA
```

---

# Summary

Troubleshooting Amazon EBS requires checking both AWS and Linux. Start by verifying the volume state and Availability Zone in AWS, then inspect the operating system for device detection, filesystem integrity, and mount configuration. Combining CloudWatch metrics, CloudTrail logs, and Linux diagnostics helps quickly identify root causes and restore production services.

---

# Related Topics

- Amazon EC2
- AWS KMS
- Amazon CloudWatch
- AWS CloudTrail
- Linux Filesystems
- Amazon EBS Snapshots

---

# References

- Amazon EBS User Guide
- AWS CLI Command Reference
- AWS Well-Architected Framework
- AWS Knowledge Center