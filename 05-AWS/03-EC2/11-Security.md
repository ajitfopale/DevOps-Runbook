# Amazon EC2 Security

> **Document Version:** 1.0
> **Category:** AWS Compute
> **Service:** Amazon EC2
> **Difficulty:** Intermediate to Advanced
> **Estimated Reading Time:** 45 Minutes

---

# Objective

After reading this document, you will understand:

- AWS Shared Responsibility Model
- IAM Roles and IAM Policies
- Security Groups
- Network ACLs
- EC2 Key Pair Security
- SSH Hardening
- IMDSv2
- EBS Encryption
- Secrets Management
- CloudTrail
- GuardDuty
- AWS Inspector
- AWS Config
- Security Best Practices
- Production Security Checklist

---

# Introduction

Security is one of the most important aspects of running Amazon EC2 in production.

A secure EC2 instance is not achieved by a single feature—it requires multiple layers of protection, including identity, networking, encryption, monitoring, and logging.

AWS follows a **Defense in Depth** strategy, where multiple security controls work together.

---

# EC2 Security Architecture

```
User

↓

IAM Authentication

↓

AWS Account

↓

VPC

↓

Network ACL

↓

Security Group

↓

EC2 Instance

↓

Operating System Firewall

↓

Application

↓

Encrypted EBS Volume
```

---

# AWS Shared Responsibility Model

AWS and the customer share responsibility for security.

## AWS is Responsible For

- Physical Data Centers
- Physical Servers
- Networking Infrastructure
- Storage Hardware
- Hypervisor
- AWS Managed Services

## Customer is Responsible For

- IAM Users
- Passwords
- Security Groups
- Network ACLs
- EC2 Operating System
- Installed Applications
- Patching
- Data
- Encryption Configuration

---

# IAM Roles

Never store AWS Access Keys inside EC2.

Instead,

Attach an IAM Role.

```
EC2

↓

IAM Role

↓

Temporary Credentials

↓

Amazon S3
```

Benefits

- Secure
- Temporary credentials
- Automatic rotation

---

# IAM Policies

Follow the Principle of Least Privilege.

Example

Application uploads files to S3.

Allow

```
s3:PutObject
```

Avoid

```
AdministratorAccess
```

---

# Security Groups

Security Groups are **stateful virtual firewalls** attached to EC2 instances.

Example

| Port | Service | Source |
|------|----------|---------|
|22|SSH|Office IP|
|80|HTTP|Internet|
|443|HTTPS|Internet|

Best Practices

- Allow only required ports
- Remove unused rules
- Restrict SSH access
- Review rules regularly

Never

```
All Traffic

0.0.0.0/0
```

---

# Network ACL (NACL)

Network ACL protects the subnet.

Security Group protects the instance.

Comparison

| Security Group | Network ACL |
|---------------|-------------|
| Stateful | Stateless |
| Instance Level | Subnet Level |
| Allow Rules | Allow & Deny Rules |

Use both together for layered security.

---

# EC2 Key Pair Security

Use SSH Key Pairs for authentication.

Best Practices

- Protect private keys
- Never email private keys
- Never upload to GitHub
- Store securely
- Rotate keys periodically

Correct Permission

```bash
chmod 400 my-key.pem
```

---

# SSH Hardening

Recommended

- Disable password authentication
- Use SSH keys only
- Restrict SSH to trusted IPs
- Change SSH port only if required by policy
- Disable root login
- Enable logging

Avoid

```
SSH

↓

0.0.0.0/0
```

---

# Systems Manager Session Manager

Instead of opening port 22,

Use

```
AWS Systems Manager

↓

Session Manager

↓

Secure Shell Access
```

Benefits

- No public SSH port
- IAM Authentication
- Session Logging
- Audit Trail

---

# Instance Metadata Service (IMDSv2)

EC2 provides metadata such as:

- Instance ID
- IAM Role Credentials
- Region
- AMI ID
- Private IP

Use **IMDSv2** instead of IMDSv1.

Benefits

- Better protection against SSRF attacks
- Session-oriented authentication
- Improved security

---

# EBS Encryption

Encrypt production EBS volumes using AWS KMS.

Protects

- Customer data
- Databases
- Backups

Encryption applies to:

- Root Volume
- Data Volumes
- Snapshots

---

# HTTPS and TLS

Never expose production applications using plain HTTP.

Use

```
HTTPS

↓

TLS Certificate

↓

Encrypted Communication
```

AWS Services

- ACM (AWS Certificate Manager)
- Application Load Balancer

---

# Secrets Management

Never store:

- Passwords
- API Keys
- Database Credentials

inside:

- Source Code
- Git Repository
- Configuration Files

Use

- AWS Secrets Manager
- AWS Systems Manager Parameter Store

---

# CloudTrail

CloudTrail records AWS API activity.

Example

```
User

↓

Terminate Instance

↓

CloudTrail Log
```

Benefits

- Auditing
- Compliance
- Security Investigation

---

# AWS Config

AWS Config continuously evaluates AWS resources.

Example

Detect

- Public Security Groups
- Unencrypted EBS Volumes
- IAM Policy Violations

---

# Amazon GuardDuty

Threat Detection Service

Detects

- Cryptocurrency mining
- Compromised credentials
- Port scanning
- Suspicious API calls

---

# Amazon Inspector

Automatically scans EC2 instances.

Detects

- Security vulnerabilities
- Missing patches
- Software issues
- CVEs

---

# Amazon CloudWatch

Monitor

- CPU
- Memory
- Disk
- Network
- Status Checks

Create alarms for:

- High CPU
- Disk Full
- Instance Failure

---

# Security Logging

Enable

- CloudTrail
- CloudWatch Logs
- VPC Flow Logs
- Application Logs

Logs help during:

- Auditing
- Troubleshooting
- Incident Response

---

# Patch Management

Keep instances updated.

Example

```bash
sudo dnf update -y
```

Production

Use

AWS Systems Manager Patch Manager

---

# Multi-Layer Security

```
IAM

↓

VPC

↓

Network ACL

↓

Security Group

↓

Operating System Firewall

↓

Application Authentication

↓

Encryption

↓

Monitoring
```

No single layer should be relied upon alone.

---

# Production Security Checklist

Before deployment:

- IAM Role attached
- Least Privilege policy
- No Access Keys stored
- Private subnet used where possible
- Security Group reviewed
- NACL configured
- IMDSv2 enabled
- SSH restricted
- Session Manager configured
- EBS encrypted
- HTTPS enabled
- Secrets stored securely
- CloudTrail enabled
- GuardDuty enabled
- Inspector enabled
- AWS Config enabled
- CloudWatch monitoring enabled
- Latest patches installed
- Backup strategy verified

---

# Real Production Example

```
Internet

↓

AWS WAF

↓

Application Load Balancer

↓

Private EC2

↓

IAM Role

↓

Encrypted EBS

↓

Amazon RDS

↓

CloudTrail

↓

GuardDuty

↓

Inspector

↓

CloudWatch
```

---

# Common Security Mistakes

❌ Using the Root AWS Account

❌ Opening SSH (22) to the Internet

❌ Using AdministratorAccess everywhere

❌ Hardcoding AWS Access Keys

❌ Not encrypting EBS volumes

❌ Ignoring security patches

❌ Disabling CloudTrail

❌ Storing secrets in GitHub

❌ Public production databases

❌ No monitoring

---

# Interview Questions

## Beginner

1. What is the AWS Shared Responsibility Model?
2. What is an IAM Role?
3. What is a Security Group?
4. What is a Network ACL?
5. Why should EBS volumes be encrypted?

---

## Intermediate

1. Explain Security Group vs NACL.
2. What is IMDSv2?
3. Why use Session Manager instead of SSH?
4. What is AWS GuardDuty?
5. What is AWS Inspector?

---

## Advanced

1. Design a secure production EC2 architecture.
2. How would you secure SSH access?
3. Explain a multi-layer EC2 security strategy.
4. How would you detect unauthorized API calls?
5. How would you manage secrets in production?

---

# Quick Revision

```
IAM

↓

Security Group

↓

NACL

↓

IMDSv2

↓

SSH Hardening

↓

Session Manager

↓

Encryption

↓

CloudTrail

↓

GuardDuty

↓

Inspector

↓

Monitoring
```

---

# Summary

Securing Amazon EC2 requires multiple layers of protection. By combining IAM Roles, Security Groups, Network ACLs, encrypted EBS volumes, IMDSv2, Systems Manager Session Manager, CloudTrail, GuardDuty, Inspector, AWS Config, and CloudWatch, organizations can build secure, auditable, and production-ready infrastructure that aligns with AWS security best practices.

---

# Related Topics

- IAM
- VPC
- Amazon EBS
- AWS KMS
- AWS Secrets Manager
- CloudTrail
- CloudWatch
- GuardDuty
- Inspector
- AWS Config

---

# References

- AWS EC2 Security Best Practices
- AWS Well-Architected Framework – Security Pillar
- AWS IAM User Guide
- AWS Security Documentation