# Amazon EBS - How It Works

> **Document Version:** 1.0  
> **Category:** AWS Storage  
> **Service:** Amazon Elastic Block Store (EBS)  
> **Difficulty:** Intermediate  
> **Estimated Reading Time:** 25–30 Minutes

---

# Objective

After reading this document, you will understand:

- How EBS works internally
- What happens when an EBS volume is created
- How EBS is attached to an EC2 instance
- Read and write data flow
- File system creation and mounting
- What happens during stop, start, reboot, and termination
- Real production workflow

---

# Introduction

When you create an EC2 instance, you don't directly interact with physical storage.

AWS automatically provisions a virtual storage device called an **EBS Volume**.

To you, it looks like a normal SSD or HDD.

Behind the scenes, AWS performs many operations to make this possible.

---

# Complete EBS Workflow

```text
User

↓

Create Volume

↓

AWS Allocates Storage

↓

Volume Created

↓

Attach to EC2

↓

Linux Detects New Disk

↓

Partition Disk (Optional)

↓

Create File System

↓

Mount Volume

↓

Read / Write Data

↓

Snapshot (Optional)

↓

Detach / Delete
```

---

# Step 1 - User Creates an EBS Volume

A user creates a volume using:

- AWS Console
- AWS CLI
- Terraform
- CloudFormation
- SDK

Example:

```
Volume Type : gp3

Size : 20 GB

Availability Zone : ap-south-1a
```

---

# Step 2 - AWS Allocates Physical Storage

AWS reserves storage inside its storage infrastructure.

Internally AWS:

- Allocates requested size
- Creates volume metadata
- Assigns a Volume ID
- Replicates storage inside the Availability Zone

Example:

```
Volume ID

vol-0ab123456789xyz
```

---

# Step 3 - Volume State

Immediately after creation:

```
Available
```

This means:

✔ Ready to attach

Not yet connected to any EC2 instance.

---

# Step 4 - Attach Volume

When you attach a volume:

AWS verifies:

- Same Region
- Same Availability Zone
- EC2 exists
- IAM permissions

If everything is correct:

AWS connects the volume to the EC2 instance.

---

# Step 5 - Linux Detects New Disk

Linux detects a new block device.

Check using:

```bash
lsblk
```

Example:

```text
xvda     20G

xvdf    100G
```

or

```bash
fdisk -l
```

At this stage:

No file system exists.

The disk is empty.

---

# Step 6 - Create File System

Linux cannot store files on a raw disk.

You must create a file system.

Example:

```bash
sudo mkfs -t ext4 /dev/xvdf
```

This creates:

```
ext4
```

Other supported file systems:

- ext4
- xfs

---

# Step 7 - Create Mount Point

Example:

```bash
sudo mkdir /data
```

This is simply an empty directory.

---

# Step 8 - Mount Volume

Example:

```bash
sudo mount /dev/xvdf /data
```

Now:

```
/data
```

is connected to the EBS volume.

Everything written inside:

```
/data
```

is stored on EBS.

---

# Step 9 - Verify Mount

Check:

```bash
df -h
```

Example:

```text
Filesystem

/dev/xvdf

100G

Mounted on

/data
```

---

# Step 10 - Read / Write Process

When an application saves data:

```text
Application

↓

Operating System

↓

File System

↓

Kernel

↓

Block Device Driver

↓

Amazon EBS

↓

AWS Storage Infrastructure
```

Reading data follows the reverse path.

---

# Step 11 - Persistence

Example:

Create file:

```bash
echo "Hello AWS" > /data/test.txt
```

Stop EC2.

Start EC2.

Run:

```bash
cat /data/test.txt
```

Output:

```
Hello AWS
```

The file still exists because the data is stored on EBS.

---

# Stop vs Reboot vs Terminate

## Reboot

```
EC2

↓

Restart Operating System
```

EBS remains attached.

Data remains.

---

## Stop

```
EC2 Powered Off

↓

EBS Detached Logically

↓

Data Remains
```

After Start:

Volume automatically reconnects.

---

## Terminate

Depends on:

```
Delete on Termination
```

Enabled

↓

Volume deleted.

Disabled

↓

Volume survives.

---

# Snapshot Workflow

```text
EBS Volume

↓

Snapshot Request

↓

AWS Copies Changed Blocks

↓

Snapshot Stored in Amazon S3

↓

Can Restore New Volume
```

Snapshots are incremental.

Only changed blocks are stored after the first snapshot.

---

# Resize Workflow

Need more storage?

Example:

20 GB

↓

Modify Volume

↓

40 GB

↓

Operating System Detects New Size

↓

Extend File System

↓

Storage Increased
```

Linux commands:

```bash
sudo growpart

sudo resize2fs

sudo xfs_growfs
```

---

# Real Production Example

Suppose a production web server stores logs on a separate EBS volume.

```
EC2

↓

Apache

↓

/logs

↓

EBS Volume

↓

Nightly Snapshot

↓

Backup
```

If the EC2 instance fails:

Launch another EC2.

↓

Create Volume from Snapshot.

↓

Attach Volume.

↓

Mount Volume.

↓

Application continues.

---

# Best Practices

✔ Separate OS and application data.

✔ Separate database storage.

✔ Schedule snapshots.

✔ Encrypt production volumes.

✔ Monitor IOPS.

✔ Tag every volume.

✔ Delete unattached volumes.

✔ Resize before storage becomes full.

---

# Common Mistakes

❌ Forgetting to create a file system.

❌ Forgetting to mount the volume.

❌ Editing `/etc/fstab` incorrectly.

❌ Deleting a volume without a snapshot.

❌ Creating the volume in a different Availability Zone.

---

# Interview Questions

## Beginner

1. What happens after an EBS volume is created?

2. Why do we create a file system?

3. Why do we mount an EBS volume?

4. Does stopping EC2 delete EBS data?

5. Which command shows attached disks?

---

## Intermediate

1. Explain the complete EBS workflow.

2. What happens internally during attachment?

3. Why must EBS and EC2 be in the same Availability Zone?

4. Explain the read/write path.

5. How does snapshot storage work?

---

# Quick Revision

```
Create Volume

↓

Attach

↓

lsblk

↓

mkfs

↓

mkdir

↓

mount

↓

df -h

↓

Use Storage

↓

Snapshot

↓

Detach
```

---

# Summary

Amazon EBS behaves like a virtual hard disk attached to an EC2 instance. AWS allocates storage, attaches it through virtualization, and presents it to Linux as a block device. Before using a new volume, you must detect the disk, create a file system, mount it, and verify it. Data remains persistent across stop/start operations, making EBS the primary storage service for EC2 workloads.

---

# Related Topics

- EC2
- AMI
- Snapshots
- Volume Types
- File Systems
- Linux Disk Management
- CloudWatch Monitoring

---

# References

- AWS EBS User Guide
- AWS EC2 Documentation
- Linux `lsblk`, `mkfs`, `mount`, `df` man pages