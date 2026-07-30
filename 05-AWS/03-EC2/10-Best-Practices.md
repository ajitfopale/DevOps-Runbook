# Amazon EC2 - Best Practices

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Intermediate to Advanced  
> **Estimated Reading Time:** 35 Minutes

---

# Objective

After reading this document, you will understand:

- EC2 production best practices
- Security recommendations
- High Availability strategies
- Performance optimization
- Cost optimization
- Monitoring and backup strategies
- Operational excellence
- Common mistakes to avoid

---

# Introduction

Launching an EC2 instance is easy.

Running hundreds of production servers securely, reliably, and cost-effectively is much more challenging.

AWS provides best practices that help organizations build:

- Highly Available applications
- Secure infrastructure
- Cost-optimized environments
- Scalable architectures

This document summarizes the most important EC2 best practices used in production.

---

# EC2 Best Practice Categories

```
Amazon EC2 Best Practices

│

├── Security

├── Networking

├── High Availability

├── Storage

├── Performance

├── Cost Optimization

├── Monitoring

├── Backup

├── Automation

└── Operations
```

---

# 1. Use IAM Roles Instead of Access Keys

Never store AWS Access Keys inside:

- Application code
- Shell scripts
- Configuration files
- Git repositories

Instead,

Attach an IAM Role to the EC2 instance.

```
EC2

↓

IAM Role

↓

Temporary Credentials

↓

AWS Services
```

Benefits

- More secure
- Automatic credential rotation
- Easier management

---

# 2. Follow the Principle of Least Privilege

Grant only the permissions required.

Example

Application only uploads files to S3.

Correct

```
s3:PutObject
```

Avoid

```
AdministratorAccess
```

---

# 3. Launch Instances Inside a VPC

Always deploy EC2 inside a properly designed VPC.

Recommended

```
VPC

↓

Public Subnet

↓

Load Balancer

↓

Private Subnet

↓

EC2

↓

Database
```

Avoid exposing application servers directly to the internet.

---

# 4. Use Security Groups Correctly

Security Groups act as stateful firewalls.

Recommendations

✔ Allow only required ports.

✔ Restrict SSH access.

✔ Remove unused rules.

Example

| Port | Access |
|------|--------|
|22|Office IP Only|
|80|Internet|
|443|Internet|

Avoid

```
0.0.0.0/0

on every port
```

---

# 5. Use Private Subnets

Production servers should normally stay in private subnets.

Example

```
Internet

↓

Application Load Balancer

↓

Private EC2

↓

RDS
```

Benefits

- Improved security
- Reduced attack surface

---

# 6. Use Auto Scaling

Never rely on a single EC2 instance.

```
CPU > 70%

↓

Auto Scaling

↓

Launch New EC2
```

Benefits

- High Availability
- Automatic scaling
- Cost optimization

---

# 7. Place a Load Balancer in Front of EC2

```
Internet

↓

Application Load Balancer

↓

EC2-1

EC2-2

EC2-3
```

Benefits

- Traffic distribution
- Fault tolerance
- Better user experience

---

# 8. Choose the Correct Instance Type

Examples

| Workload | Instance |
|-----------|----------|
|Development|t3.micro|
|Web Server|m6i.large|
|Database|r6i.large|
|Machine Learning|p5|

Monitor utilization and resize if necessary.

---

# 9. Prefer New Generation Instances

Example

Instead of

```
m4
```

Use

```
m7g

or

m7i
```

Benefits

- Better performance
- Lower cost
- Improved efficiency

---

# 10. Use gp3 EBS Volumes

Recommended for most workloads.

Advantages

- Better performance
- Lower cost
- Independent IOPS
- Independent throughput

---

# 11. Enable Detailed Monitoring

Use Amazon CloudWatch.

Monitor

- CPU
- Memory (CloudWatch Agent)
- Disk
- Network
- Application Logs

Create alarms for:

- High CPU
- Low Disk Space
- Instance Status Check Failure

---

# 12. Enable Logging

Recommended services

- CloudWatch Logs
- CloudTrail
- VPC Flow Logs

Never troubleshoot production systems without logs.

---

# 13. Keep Operating Systems Updated

Regularly install:

- Security patches
- Kernel updates
- Package updates

Example

```bash
sudo dnf update -y
```

Automate patching using:

- AWS Systems Manager Patch Manager

---

# 14. Tag Every Resource

Example

| Key | Value |
|------|-------|
|Name|WebServer01|
|Environment|Production|
|Project|Banking|
|Owner|DevOps|

Benefits

- Cost tracking
- Automation
- Easy identification

---

# 15. Create AMIs

Create AMIs before

- Major upgrades
- OS changes
- Software deployments

Benefits

Fast rollback.

---

# 16. Take Regular EBS Snapshots

Use snapshots for:

- Backup
- Disaster Recovery
- Migration

Recommended

- Daily Snapshot
- Weekly Verification
- Monthly Retention Review

---

# 17. Encrypt EBS Volumes

Always encrypt production storage.

Protects

- Customer Data
- Business Data
- Backups

Use AWS KMS-managed encryption.

---

# 18. Use Systems Manager Session Manager

Instead of exposing SSH publicly.

Benefits

- No public SSH access
- Audited sessions
- IAM authentication
- Improved security

---

# 19. Use Elastic IP Carefully

Allocate Elastic IPs only when necessary.

Release unused Elastic IPs to avoid unnecessary charges.

---

# 20. Monitor Costs

Use

- AWS Cost Explorer
- AWS Budgets
- Cost Anomaly Detection

Review costs regularly.

---

# Production Deployment Checklist

Before deploying an EC2 instance to production:

- IAM Role attached
- Security Groups reviewed
- Private Subnet used where possible
- Latest AMI selected
- Correct instance type chosen
- gp3 root volume configured
- Encryption enabled
- CloudWatch monitoring enabled
- CloudTrail enabled
- Tags added
- Backup strategy defined
- Auto Scaling configured
- Load Balancer configured
- Health checks verified
- Cost reviewed

---

# Real Production Example

```
Internet

↓

Route 53

↓

AWS WAF

↓

Application Load Balancer

↓

Auto Scaling Group

↓

EC2 Instances

↓

Amazon RDS

↓

Amazon S3

↓

CloudWatch

↓

CloudTrail

↓

AWS Backup
```

This architecture follows AWS Well-Architected principles.

---

# Common Mistakes

❌ Using the root account.

❌ Storing AWS keys on EC2.

❌ Opening SSH to everyone.

❌ Running production on one EC2 instance.

❌ Forgetting backups.

❌ No monitoring.

❌ No tagging.

❌ Using outdated AMIs.

❌ Oversized instances.

❌ No disaster recovery plan.

---

# Interview Questions

## Beginner

1. What are EC2 best practices?
2. Why should you use IAM Roles?
3. Why should production EC2 instances be placed in private subnets?
4. Why is tagging important?
5. Why should you use Auto Scaling?

---

## Intermediate

1. Explain the principle of least privilege.
2. Why is gp3 recommended over gp2?
3. Why should you use Systems Manager Session Manager?
4. How do you monitor EC2 instances?
5. Explain a production deployment checklist.

---

# Quick Revision

```
IAM Role

↓

Private Subnet

↓

Security Group

↓

Load Balancer

↓

Auto Scaling

↓

CloudWatch

↓

CloudTrail

↓

Snapshots

↓

AMI

↓

Encryption

↓

Monitoring

↓

Backup
```

---

# Summary

Building production-ready EC2 environments requires much more than launching virtual machines. By following AWS best practices—such as using IAM Roles, private subnets, Auto Scaling, Load Balancers, encryption, monitoring, backups, tagging, and automation—you can build secure, highly available, scalable, and cost-effective infrastructure that aligns with the AWS Well-Architected Framework.

---

# Related Topics

- IAM
- VPC
- Auto Scaling
- Elastic Load Balancer
- Amazon EBS
- CloudWatch
- CloudTrail
- AWS Systems Manager
- AWS Backup

---

# References

- AWS EC2 Best Practices
- AWS Well-Architected Framework
- AWS Security Best Practices