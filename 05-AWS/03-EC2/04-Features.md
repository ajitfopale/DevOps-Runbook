# Amazon EC2 - How It Works

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2 (Elastic Compute Cloud)  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 30 Minutes

---

# Objective

After reading this document, you will understand:

- What happens when an EC2 instance is launched
- How AWS creates a virtual machine
- The EC2 boot process
- The relationship between EC2, AMI, EBS, VPC, and Security Groups
- Instance lifecycle
- Real production workflow

---

# Introduction

Launching an EC2 instance is much more than clicking the **Launch Instance** button.

Behind the scenes, AWS performs multiple operations:

- Authenticates the user
- Validates permissions
- Allocates compute resources
- Creates storage
- Configures networking
- Boots the operating system
- Makes the instance available

All of these steps happen in a few minutes or less.

---

# Complete EC2 Launch Workflow

```text
User

↓

AWS Console / CLI / API

↓

IAM Authentication

↓

EC2 Control Plane

↓

Select Region

↓

Select Availability Zone

↓

Select AMI

↓

Select Instance Type

↓

Create / Attach EBS Volume

↓

Configure VPC

↓

Configure Subnet

↓

Assign Private IP

↓

(Optional) Assign Public IP

↓

Attach Security Group

↓

Launch Instance on Physical Host

↓

Operating System Boots

↓

Instance Running

↓

SSH / RDP Access
```

---

# Step 1 - User Sends Request

The process starts when a user launches an EC2 instance using:

- AWS Management Console
- AWS CLI
- SDK
- Terraform
- CloudFormation

Example CLI:

```bash
aws ec2 run-instances \
--image-id ami-xxxxxxxx \
--instance-type t3.micro
```

---

# Step 2 - IAM Authorization

AWS checks:

- Is the user authenticated?
- Does the user have `ec2:RunInstances` permission?
- Is access allowed in this Region?

If any check fails, the request is rejected.

---

# Step 3 - EC2 Control Plane

The EC2 control plane coordinates instance creation.

It communicates with:

- Amazon EBS
- Amazon VPC
- IAM
- Placement service
- Monitoring services

---

# Step 4 - Choose an AMI

An Amazon Machine Image (AMI) acts as a template.

It contains:

- Operating System
- Boot configuration
- Installed software
- Root volume configuration

Example:

```
Amazon Linux 2023

↓

Kernel

↓

Packages

↓

Default Configuration
```

---

# Step 5 - Choose an Instance Type

AWS allocates CPU and memory based on the selected instance type.

Examples:

| Instance | vCPU | Memory |
|----------|-----:|-------:|
| t2.micro | 1 | 1 GB |
| t3.micro | 2 | 1 GB |
| m5.large | 2 | 8 GB |
| c6i.large | 2 | 4 GB |

---

# Step 6 - Allocate Physical Resources

AWS selects a physical host inside the chosen Availability Zone.

Using the AWS virtualization platform (Nitro System for modern instances), it creates an isolated virtual machine.

The user never sees the physical server.

---

# Step 7 - Create or Attach Root EBS Volume

AWS automatically creates the root EBS volume from the selected AMI.

Example:

```text
AMI

↓

20 GB gp3 Root Volume

↓

Attached as /dev/xvda
```

Additional EBS volumes can also be attached.

---

# Step 8 - Configure Networking

AWS configures networking inside your VPC.

Resources include:

- VPC
- Subnet
- Route Table
- Network Interface (ENI)
- Private IP Address

Example:

```text
VPC

↓

Public Subnet

↓

EC2

↓

Private IP

10.0.1.15
```

---

# Step 9 - Security Group

A Security Group acts as a virtual firewall.

Example:

| Port | Purpose |
|------|---------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |

Traffic is allowed or denied based on these rules.

---

# Step 10 - Public IP Assignment

If enabled:

AWS assigns a public IPv4 address.

Example:

```text
Private IP

10.0.1.15

↓

Public IP

13.xxx.xxx.xxx
```

Without a public IP (or another access method such as a bastion host or Systems Manager), you cannot connect directly from the internet.

---

# Step 11 - Boot Process

The instance powers on.

The operating system:

- Detects CPU
- Detects RAM
- Detects EBS volumes
- Loads kernel
- Starts system services
- Starts networking

Linux reaches:

```text
multi-user.target
```

After boot, SSH becomes available.

---

# Step 12 - User Connects

Linux:

```bash
ssh -i my-key.pem ec2-user@Public-IP
```

Windows:

Use Remote Desktop (RDP).

---

# Data Flow

```text
User

↓

SSH

↓

EC2 Operating System

↓

Application

↓

File System

↓

Amazon EBS
```

---

# EC2 Lifecycle

```text
Pending

↓

Running

↓

Stopping

↓

Stopped

↓

Starting

↓

Running

↓

Terminated
```

### Pending

Resources are being allocated.

### Running

The instance is ready for use.

### Stopped

Compute charges stop (storage charges for EBS continue).

### Terminated

The instance is permanently deleted.

Whether the root EBS volume is deleted depends on the **Delete on Termination** setting.

---

# Real Production Example

An online shopping application:

```text
Internet

↓

Application Load Balancer

↓

EC2-1        EC2-2        EC2-3

↓

Amazon RDS

↓

Amazon S3
```

When traffic increases:

- Auto Scaling launches additional EC2 instances.
- New instances are created using the same AMI.
- They automatically register with the Load Balancer.

---

# Best Practices

- Use IAM Roles instead of storing AWS keys.
- Use Security Groups with least privilege.
- Keep production servers in private subnets where possible.
- Use gp3 for root volumes.
- Enable detailed monitoring when needed.
- Regularly patch your AMIs.
- Tag all instances consistently.

---

# Common Mistakes

- Launching in the wrong Region.
- Choosing the wrong instance type.
- Forgetting to open SSH or RDP in the Security Group.
- Losing the key pair.
- Deleting an instance without checking the root volume deletion setting.
- Using the root AWS account for daily work.

---

# Interview Questions

## Beginner

1. What happens when you launch an EC2 instance?
2. What is the role of an AMI?
3. What is the function of a Security Group?
4. What is the difference between a private IP and a public IP?
5. Why does EC2 need an EBS volume?

## Intermediate

1. Explain the EC2 launch process step by step.
2. What is the EC2 control plane?
3. What is the relationship between EC2 and VPC?
4. How does EC2 communicate with EBS?
5. Explain the EC2 lifecycle.

---

# Quick Revision

```text
Launch Request

↓

IAM

↓

AMI

↓

Instance Type

↓

Physical Host

↓

EBS

↓

VPC

↓

Security Group

↓

Boot

↓

Running

↓

SSH
```

---

# Summary

Amazon EC2 is a managed compute service that creates virtual machines on AWS infrastructure. Behind every EC2 launch, AWS validates permissions, provisions compute, attaches storage, configures networking, applies security settings, boots the operating system, and makes the instance available. Understanding this workflow helps in designing, troubleshooting, and operating production workloads.

---

# Related Topics

- AMI
- EBS
- VPC
- Security Groups
- IAM
- Auto Scaling
- Elastic Load Balancer
- CloudWatch

---

# References

- AWS EC2 User Guide: https://docs.aws.amazon.com/ec2/
- AWS EC2 FAQs: https://aws.amazon.com/ec2/faqs/