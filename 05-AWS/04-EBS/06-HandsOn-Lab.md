# Amazon EBS Hands-On Lab

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 45 Minutes

---

# Objective

In this lab, you will learn how to:

- Create an EBS Volume
- Attach it to an EC2 Instance
- Detect the new disk in Linux
- Create a filesystem
- Mount the volume
- Configure persistent mounting
- Resize the EBS volume
- Extend the filesystem
- Create snapshots
- Restore a volume from a snapshot
- Troubleshoot common issues

---

# Lab Architecture

```
AWS Console

↓

EC2 Instance

↓

Attach EBS Volume

↓

Linux

↓

Format

↓

Mount

↓

Store Data

↓

Snapshot

↓

Restore
```

---

# Lab Prerequisites

Before starting, ensure you have:

- AWS Account
- Running EC2 Instance
- SSH Access
- IAM permissions for EC2 and EBS
- Amazon Linux 2 / Amazon Linux 2023 / RHEL / Ubuntu

---

# Lab 1: Create an EBS Volume

## Step 1

AWS Console

↓

EC2

↓

Elastic Block Store

↓

Volumes

↓

Create Volume

---

## Configure

Example

| Setting | Value |
|----------|-------|
| Volume Type | gp3 |
| Size | 10 GB |
| Availability Zone | Same as EC2 |
| Encryption | Enabled |

Click **Create Volume**.

---

# Lab 2: Attach Volume

Select the volume.

↓

Actions

↓

Attach Volume

↓

Select EC2 Instance

↓

Device Name

Example

```
/dev/xvdf
```

Click **Attach**.

---

# Lab 3: Verify Volume in Linux

SSH into the instance.

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Check available block devices.

```bash
lsblk
```

Example

```
NAME         SIZE

nvme0n1      8G

nvme1n1      10G
```

Another useful command

```bash
sudo fdisk -l
```

---

# Lab 4: Create a Partition (Optional)

If using the entire disk directly, this step can be skipped.

Create a partition.

```bash
sudo fdisk /dev/nvme1n1
```

Commands inside `fdisk`

```
n

p

1

Enter

Enter

w
```

Verify

```bash
lsblk
```

Expected

```
nvme1n1

└── nvme1n1p1
```

---

# Lab 5: Create Filesystem

Format the partition.

```bash
sudo mkfs -t xfs /dev/nvme1n1p1
```

or

```bash
sudo mkfs.ext4 /dev/nvme1n1p1
```

Verify

```bash
sudo blkid
```

---

# Lab 6: Create Mount Point

```bash
sudo mkdir /data
```

Verify

```bash
ls /
```

---

# Lab 7: Mount the Volume

```bash
sudo mount /dev/nvme1n1p1 /data
```

Check

```bash
df -h
```

Expected output

```
Filesystem

Mounted on

/data
```

---

# Lab 8: Test Storage

Move into the mounted directory.

```bash
cd /data
```

Create a test file.

```bash
echo "Amazon EBS Lab" > test.txt
```

Verify

```bash
cat test.txt
```

---

# Lab 9: Configure Persistent Mount

Without configuration, the volume will not mount automatically after a reboot.

Find UUID.

```bash
sudo blkid
```

Example

```
UUID=1234-abcd
```

Edit `/etc/fstab`.

```bash
sudo vi /etc/fstab
```

Example entry

```
UUID=1234-abcd   /data   xfs   defaults,nofail   0   2
```

Test configuration.

```bash
sudo mount -a
```

If there is no error, the configuration is correct.

---

# Lab 10: Increase Volume Size

AWS Console

↓

Volumes

↓

Modify Volume

↓

Increase

```
10 GB

↓

20 GB
```

Wait until the modification is completed.

---

# Lab 11: Verify New Size

```bash
lsblk
```

Example

```
20G
```

The filesystem still uses the old size.

---

# Lab 12: Extend the Filesystem

For XFS

```bash
sudo xfs_growfs /data
```

For EXT4

```bash
sudo resize2fs /dev/nvme1n1p1
```

Verify

```bash
df -h
```

The new capacity should now be visible.

---

# Lab 13: Create a Snapshot

AWS Console

↓

Volumes

↓

Actions

↓

Create Snapshot

Provide

- Name
- Description
- Tags

Click **Create Snapshot**.

---

# Lab 14: Restore Volume

Snapshot

↓

Create Volume

↓

Attach Volume

↓

Mount

↓

Verify Data

Your files should be restored.

---

# Lab 15: Detach Volume

Before detaching

```bash
sudo umount /data
```

AWS Console

↓

Detach Volume

Never detach an actively used volume without unmounting it first.

---

# Common Linux Commands

Check disks

```bash
lsblk
```

Filesystem

```bash
df -h
```

Disk usage

```bash
du -sh /data
```

Filesystem UUID

```bash
blkid
```

Mounted filesystems

```bash
mount
```

Unmount

```bash
umount /data
```

---

# Troubleshooting

## Wrong Filesystem Type

```
wrong fs type

bad superblock
```

Possible causes

- Wrong partition
- Filesystem not created
- Corrupted filesystem

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

## Device Not Found

Verify

- Volume attached
- Correct Availability Zone
- Correct device name

Run

```bash
lsblk
```

---

## Mount Lost After Reboot

Most common cause

Incorrect `/etc/fstab`.

Test before reboot

```bash
sudo mount -a
```

---

# Production Best Practices

- Use gp3 for most workloads
- Encrypt all EBS volumes
- Tag every volume
- Use UUID in `/etc/fstab`
- Take snapshots before major changes
- Test restore procedures regularly
- Monitor storage usage with CloudWatch
- Resize volumes before they become full
- Detach volumes only after unmounting

---

# Lab Checklist

- Created EBS Volume
- Attached to EC2
- Verified using `lsblk`
- Created filesystem
- Mounted volume
- Tested read/write
- Configured `/etc/fstab`
- Expanded volume
- Extended filesystem
- Created snapshot
- Restored snapshot
- Detached volume safely

---

# Interview Questions

1. How do you attach an EBS volume to EC2?
2. Why do we use `lsblk`?
3. What is the purpose of `mkfs`?
4. Why should you use UUID in `/etc/fstab`?
5. How do you extend an EBS filesystem after increasing the volume size?
6. What happens if `/etc/fstab` is configured incorrectly?
7. How do you restore data using an EBS snapshot?
8. What is the difference between `df -h` and `du -sh`?
9. Why should you unmount a volume before detaching it?
10. What is the safest way to test an `/etc/fstab` entry?

---

# Summary

This hands-on lab demonstrated the complete lifecycle of an Amazon EBS volume—from creation and attachment to formatting, mounting, persistent configuration, resizing, snapshot creation, and restoration. These are core operational tasks for Linux administrators, AWS engineers, and DevOps professionals working in production environments.

---

# Related Topics

- Amazon EC2
- Amazon EBS Snapshots
- AWS Backup
- Linux Filesystems (XFS, EXT4)
- CloudWatch