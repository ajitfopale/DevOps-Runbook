# Amazon EC2 - AWS CLI Commands

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Beginner to Advanced  
> **Estimated Reading Time:** 45 Minutes

---

# Objective

After completing this guide, you will be able to:

- Configure AWS CLI
- Launch and manage EC2 instances
- Manage Key Pairs
- Work with Security Groups
- Create and manage EBS volumes
- Create AMIs
- Manage Elastic IPs
- Tag AWS resources
- Troubleshoot EC2 using AWS CLI

---

# What is AWS CLI?

AWS CLI (Command Line Interface) is a tool that allows you to interact with AWS services directly from the terminal.

Instead of using the AWS Management Console, you can execute commands to automate AWS operations.

---

# Install AWS CLI

### Windows

Download and install AWS CLI from the official AWS website.

Verify installation:

```bash
aws --version
```

Example Output

```bash
aws-cli/2.27.45 Python/3.x Windows/11
```

---

# Configure AWS CLI

```bash
aws configure
```

Enter:

```text
AWS Access Key ID:

AWS Secret Access Key:

Default Region:

Default Output Format:
```

Example

```text
Region

ap-south-1

Output

json
```

Verify

```bash
aws sts get-caller-identity
```

---

# Region Commands

Check current region

```bash
aws configure get region
```

List available regions

```bash
aws ec2 describe-regions
```

---

# Availability Zones

```bash
aws ec2 describe-availability-zones
```

---

# AMI Commands

List Amazon Linux AMIs

```bash
aws ec2 describe-images \
--owners amazon
```

Describe a specific AMI

```bash
aws ec2 describe-images \
--image-ids ami-xxxxxxxx
```

Create AMI from an EC2 instance

```bash
aws ec2 create-image \
--instance-id i-xxxxxxxx \
--name "My-AMI"
```

---

# Launch EC2 Instance

```bash
aws ec2 run-instances \
--image-id ami-xxxxxxxx \
--instance-type t3.micro \
--key-name MyKey \
--security-group-ids sg-xxxxxxxx \
--subnet-id subnet-xxxxxxxx
```

---

# Describe Instances

```bash
aws ec2 describe-instances
```

Specific instance

```bash
aws ec2 describe-instances \
--instance-ids i-xxxxxxxx
```

Only Running Instances

```bash
aws ec2 describe-instances \
--filters Name=instance-state-name,Values=running
```

---

# Start Instance

```bash
aws ec2 start-instances \
--instance-ids i-xxxxxxxx
```

---

# Stop Instance

```bash
aws ec2 stop-instances \
--instance-ids i-xxxxxxxx
```

---

# Reboot Instance

```bash
aws ec2 reboot-instances \
--instance-ids i-xxxxxxxx
```

---

# Terminate Instance

```bash
aws ec2 terminate-instances \
--instance-ids i-xxxxxxxx
```

---

# Wait Until Running

```bash
aws ec2 wait instance-running \
--instance-ids i-xxxxxxxx
```

---

# Key Pair Commands

Create Key Pair

```bash
aws ec2 create-key-pair \
--key-name DevOpsKey
```

List Key Pairs

```bash
aws ec2 describe-key-pairs
```

Delete Key Pair

```bash
aws ec2 delete-key-pair \
--key-name DevOpsKey
```

---

# Security Group Commands

List Security Groups

```bash
aws ec2 describe-security-groups
```

Create Security Group

```bash
aws ec2 create-security-group \
--group-name WebServerSG \
--description "Web Server SG" \
--vpc-id vpc-xxxxxxxx
```

Allow SSH

```bash
aws ec2 authorize-security-group-ingress \
--group-id sg-xxxxxxxx \
--protocol tcp \
--port 22 \
--cidr 0.0.0.0/0
```

Allow HTTP

```bash
aws ec2 authorize-security-group-ingress \
--group-id sg-xxxxxxxx \
--protocol tcp \
--port 80 \
--cidr 0.0.0.0/0
```

---

# EBS Volume Commands

Create Volume

```bash
aws ec2 create-volume \
--availability-zone ap-south-1a \
--size 10 \
--volume-type gp3
```

Describe Volumes

```bash
aws ec2 describe-volumes
```

Attach Volume

```bash
aws ec2 attach-volume \
--volume-id vol-xxxxxxxx \
--instance-id i-xxxxxxxx \
--device /dev/xvdf
```

Detach Volume

```bash
aws ec2 detach-volume \
--volume-id vol-xxxxxxxx
```

Delete Volume

```bash
aws ec2 delete-volume \
--volume-id vol-xxxxxxxx
```

---

# Snapshot Commands

Create Snapshot

```bash
aws ec2 create-snapshot \
--volume-id vol-xxxxxxxx
```

Describe Snapshots

```bash
aws ec2 describe-snapshots \
--owner-ids self
```

Delete Snapshot

```bash
aws ec2 delete-snapshot \
--snapshot-id snap-xxxxxxxx
```

---

# Elastic IP Commands

Allocate Elastic IP

```bash
aws ec2 allocate-address
```

Associate Elastic IP

```bash
aws ec2 associate-address \
--instance-id i-xxxxxxxx \
--allocation-id eipalloc-xxxxxxxx
```

Release Elastic IP

```bash
aws ec2 release-address \
--allocation-id eipalloc-xxxxxxxx
```

---

# Tag Resources

Create Tags

```bash
aws ec2 create-tags \
--resources i-xxxxxxxx \
--tags Key=Name,Value=WebServer
```

Describe Tags

```bash
aws ec2 describe-tags
```

---

# Monitoring Commands

Monitor Instance

```bash
aws ec2 monitor-instances \
--instance-ids i-xxxxxxxx
```

Disable Monitoring

```bash
aws ec2 unmonitor-instances \
--instance-ids i-xxxxxxxx
```

---

# Useful Filters

Running Instances

```bash
aws ec2 describe-instances \
--filters Name=instance-state-name,Values=running
```

Filter by Tag

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=WebServer
```

---

# AWS CLI Output Formats

JSON

```bash
aws ec2 describe-instances --output json
```

Table

```bash
aws ec2 describe-instances --output table
```

Text

```bash
aws ec2 describe-instances --output text
```

---

# Useful AWS CLI Commands

Current Identity

```bash
aws sts get-caller-identity
```

Current Region

```bash
aws configure get region
```

Current Configuration

```bash
aws configure list
```

---

# Best Practices

- Use IAM Roles instead of storing access keys on EC2.
- Restrict IAM permissions using the principle of least privilege.
- Use tags for easy resource management.
- Prefer `--filters` to reduce unnecessary output.
- Avoid using the root AWS account with the CLI.
- Rotate access keys regularly if using IAM users.

---

# Common Mistakes

❌ Running commands in the wrong AWS Region.

❌ Forgetting the `--instance-ids` parameter.

❌ Using an incorrect AMI ID.

❌ Creating resources without tags.

❌ Exposing Security Groups to `0.0.0.0/0` unnecessarily.

---

# Interview Questions

## Beginner

1. What is AWS CLI?
2. How do you configure AWS CLI?
3. Which command launches an EC2 instance?
4. How do you stop an EC2 instance?
5. How do you list all EC2 instances?

---

## Intermediate

1. Explain `aws configure`.
2. How do you filter running EC2 instances?
3. How do you create an AMI using AWS CLI?
4. How do you attach an EBS volume using AWS CLI?
5. How do you associate an Elastic IP?

---

# Quick Revision

```text
AWS CLI

↓

aws configure

↓

Launch EC2

↓

Manage Instances

↓

Manage EBS

↓

Manage Security Groups

↓

Manage Snapshots

↓

Manage Elastic IP

↓

Tag Resources

↓

Monitor Resources
```

---

# Summary

AWS CLI provides a powerful and scriptable way to manage Amazon EC2 and related AWS services. It enables automation, infrastructure management, and day-to-day operations without using the AWS Management Console. Learning the most common EC2 CLI commands is an essential skill for DevOps Engineers, Cloud Engineers, and System Administrators.

---

# Related Topics

- EC2 Instance Types
- Amazon EBS
- Security Groups
- IAM
- AWS CloudShell
- Auto Scaling
- CloudFormation
- Terraform

---

# References

- AWS CLI User Guide: https://docs.aws.amazon.com/cli/
- EC2 CLI Reference: https://docs.aws.amazon.com/cli/latest/reference/ec2/