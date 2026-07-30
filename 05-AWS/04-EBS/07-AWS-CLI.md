# Amazon EBS AWS CLI Guide

> **Document Version:** 1.0
> **Category:** AWS Storage
> **Service:** Amazon Elastic Block Store (EBS)
> **Difficulty:** Intermediate
> **Estimated Reading Time:** 40 Minutes

---

# Objective

After reading this document, you will learn how to manage Amazon EBS using the AWS CLI.

Topics covered:

- Configure AWS CLI
- Create EBS Volumes
- Describe Volumes
- Attach Volumes
- Detach Volumes
- Modify Volumes
- Delete Volumes
- Create Snapshots
- Restore from Snapshots
- Copy Snapshots
- Tag Resources
- Wait Commands
- Filtering & Querying

---

# Prerequisites

Install AWS CLI.

Verify installation.

```bash
aws --version
```

Configure credentials.

```bash
aws configure
```

Example

```
AWS Access Key ID:
AWS Secret Access Key:
Default Region:
Output Format:
```

Verify identity.

```bash
aws sts get-caller-identity
```

---

# List EBS Volumes

```bash
aws ec2 describe-volumes
```

List only Volume IDs.

```bash
aws ec2 describe-volumes \
--query "Volumes[*].VolumeId"
```

Table output.

```bash
aws ec2 describe-volumes \
--output table
```

---

# Describe a Specific Volume

```bash
aws ec2 describe-volumes \
--volume-ids vol-0123456789abcdef0
```

---

# Create an EBS Volume

```bash
aws ec2 create-volume \
--availability-zone ap-south-1a \
--size 20 \
--volume-type gp3
```

Example Output

```
VolumeId

vol-0123456789abcdef0
```

---

# Create an Encrypted Volume

```bash
aws ec2 create-volume \
--availability-zone ap-south-1a \
--size 50 \
--volume-type gp3 \
--encrypted
```

Using a customer-managed KMS key.

```bash
aws ec2 create-volume \
--availability-zone ap-south-1a \
--size 100 \
--volume-type gp3 \
--encrypted \
--kms-key-id alias/my-ebs-key
```

---

# Attach Volume to EC2

```bash
aws ec2 attach-volume \
--volume-id vol-0123456789abcdef0 \
--instance-id i-0123456789abcdef0 \
--device /dev/xvdf
```

---

# Check Attachment Status

```bash
aws ec2 describe-volumes \
--volume-ids vol-0123456789abcdef0
```

---

# Detach Volume

```bash
aws ec2 detach-volume \
--volume-id vol-0123456789abcdef0
```

Force detach (use only if necessary).

```bash
aws ec2 detach-volume \
--volume-id vol-0123456789abcdef0 \
--force
```

---

# Modify Volume Size

Increase volume from 20 GB to 50 GB.

```bash
aws ec2 modify-volume \
--volume-id vol-0123456789abcdef0 \
--size 50
```

Remember to extend the filesystem inside Linux after resizing.

---

# Modify Volume Type

Convert gp2 to gp3.

```bash
aws ec2 modify-volume \
--volume-id vol-0123456789abcdef0 \
--volume-type gp3
```

---

# Create Snapshot

```bash
aws ec2 create-snapshot \
--volume-id vol-0123456789abcdef0 \
--description "Daily Backup"
```

---

# List Snapshots

```bash
aws ec2 describe-snapshots \
--owner-ids self
```

---

# Create Volume from Snapshot

```bash
aws ec2 create-volume \
--snapshot-id snap-0123456789abcdef0 \
--availability-zone ap-south-1a
```

---

# Copy Snapshot to Another Region

```bash
aws ec2 copy-snapshot \
--source-region ap-south-1 \
--source-snapshot-id snap-0123456789abcdef0 \
--destination-region us-east-1 \
--description "Cross Region Backup"
```

---

# Delete Snapshot

```bash
aws ec2 delete-snapshot \
--snapshot-id snap-0123456789abcdef0
```

---

# Delete Volume

```bash
aws ec2 delete-volume \
--volume-id vol-0123456789abcdef0
```

The volume must not be attached to an instance.

---

# Add Tags

```bash
aws ec2 create-tags \
--resources vol-0123456789abcdef0 \
--tags Key=Name,Value=DatabaseVolume
```

Multiple tags.

```bash
aws ec2 create-tags \
--resources vol-0123456789abcdef0 \
--tags Key=Environment,Value=Production \
Key=Owner,Value=DevOps
```

---

# Filter Volumes

Available volumes only.

```bash
aws ec2 describe-volumes \
--filters Name=status,Values=available
```

gp3 volumes only.

```bash
aws ec2 describe-volumes \
--filters Name=volume-type,Values=gp3
```

---

# Useful Query Examples

Volume IDs.

```bash
aws ec2 describe-volumes \
--query "Volumes[*].VolumeId"
```

Volume Size.

```bash
aws ec2 describe-volumes \
--query "Volumes[*].[VolumeId,Size]"
```

Volume Type.

```bash
aws ec2 describe-volumes \
--query "Volumes[*].[VolumeId,VolumeType]"
```

---

# Wait Commands

Wait until volume becomes available.

```bash
aws ec2 wait volume-available \
--volume-ids vol-0123456789abcdef0
```

Wait until snapshot completes.

```bash
aws ec2 wait snapshot-completed \
--snapshot-ids snap-0123456789abcdef0
```

---

# Monitor Volume Modifications

```bash
aws ec2 describe-volumes-modifications
```

---

# Production Example

Automated backup workflow.

```
EC2

↓

EBS Volume

↓

AWS CLI

↓

Create Snapshot

↓

Tag Snapshot

↓

Copy to DR Region

↓

Delete Old Snapshots
```

---

# Best Practices

- Prefer gp3 volumes.
- Encrypt production volumes.
- Tag every volume and snapshot.
- Automate snapshots.
- Monitor modification progress.
- Delete unused snapshots.
- Use wait commands in automation scripts.
- Validate restores regularly.

---

# Common Mistakes

❌ Creating volumes in a different Availability Zone than the EC2 instance.

❌ Forgetting to extend the filesystem after increasing volume size.

❌ Deleting snapshots without verifying backup retention.

❌ Using force detach unnecessarily.

❌ Leaving unattached volumes running and incurring charges.

---

# Interview Questions

## Beginner

1. Which AWS CLI command lists EBS volumes?
2. How do you create an EBS volume using AWS CLI?
3. How do you attach a volume to an EC2 instance?
4. How do you create an EBS snapshot?
5. How do you delete a volume?

---

## Intermediate

1. How do you modify an EBS volume type?
2. How do you create an encrypted EBS volume?
3. What is the purpose of `aws ec2 wait` commands?
4. How do you copy snapshots across Regions?
5. How do you filter gp3 volumes using AWS CLI?

---

# Quick Revision

```
aws configure

↓

describe-volumes

↓

create-volume

↓

attach-volume

↓

modify-volume

↓

create-snapshot

↓

copy-snapshot

↓

delete-snapshot

↓

delete-volume
```

---

# Summary

The AWS CLI provides complete control over Amazon EBS, allowing administrators to automate storage management, create and modify volumes, manage snapshots, apply tags, and integrate EBS operations into DevOps pipelines. Mastering these commands is essential for infrastructure automation and production operations.

---

# Related Topics

- Amazon EC2
- Amazon EBS Snapshots
- AWS Backup
- AWS CLI
- CloudWatch
- AWS Systems Manager

---

# References

- AWS CLI Command Reference
- Amazon EBS User Guide
- AWS CLI User Guide