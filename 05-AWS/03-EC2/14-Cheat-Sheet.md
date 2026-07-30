# Amazon EC2 Cheat Sheet

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Purpose:** Quick Revision Guide  
> **Estimated Reading Time:** 10–15 Minutes

---

# What is Amazon EC2?

Amazon Elastic Compute Cloud (EC2) is a web service that provides secure, resizable virtual servers in the AWS Cloud.

Use Cases

- Web Hosting
- Application Servers
- Database Servers
- DevOps Tools
- CI/CD Pipelines
- Machine Learning
- Big Data
- Containers

---

# EC2 Workflow

```
AWS Account
      │
      ▼
Select Region
      │
      ▼
Choose AMI
      │
      ▼
Select Instance Type
      │
      ▼
Configure Network (VPC/Subnet)
      │
      ▼
Attach Storage (EBS)
      │
      ▼
Configure Security Group
      │
      ▼
Create Key Pair
      │
      ▼
Launch EC2 Instance
      │
      ▼
Connect via SSH/RDP
```

---

# EC2 Lifecycle

```
Pending
   │
   ▼
Running
   │
   ├── Reboot
   │
   ├── Stop
   │      │
   │      ▼
   │   Stopped
   │      │
   │      ▼
   │   Start
   │
   ▼
Terminate
```

---

# Instance Families

| Family | Purpose |
|---------|---------|
| T | Burstable General Purpose |
| M | General Purpose |
| C | Compute Optimized |
| R | Memory Optimized |
| X | High Memory |
| I | Storage Optimized |
| D | Dense Storage |
| H | HDD Storage |
| G | Graphics |
| P | GPU / AI |
| Inf | Machine Learning Inference |
| Trn | Machine Learning Training |

---

# Storage Options

| Storage | Use Case |
|----------|----------|
| EBS | Persistent Block Storage |
| Instance Store | Temporary Storage |
| EFS | Shared File Storage |
| S3 | Object Storage |

---

# EBS Volume Types

| Type | Best For |
|------|----------|
| gp3 | General Purpose (Recommended) |
| gp2 | Legacy General Purpose |
| io2 | High Performance Databases |
| io1 | Provisioned IOPS |
| st1 | Throughput Optimized HDD |
| sc1 | Cold HDD |

---

# Pricing Models

| Model | Best For |
|--------|----------|
| On-Demand | Short-term workloads |
| Reserved Instances | Predictable workloads |
| Savings Plans | Long-term savings |
| Spot Instances | Fault-tolerant workloads |
| Dedicated Hosts | Compliance and licensing |

---

# Common Ports

| Port | Service |
|------|----------|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 1433 | SQL Server |
| 1521 | Oracle |
| 8080 | Tomcat |
| 9090 | Prometheus |
| 3000 | Grafana |

---

# Security Checklist

- Use IAM Roles
- Follow Least Privilege
- Restrict SSH access
- Use Security Groups
- Use Network ACLs
- Encrypt EBS Volumes
- Enable IMDSv2
- Use Session Manager
- Enable HTTPS
- Store secrets in AWS Secrets Manager
- Enable CloudTrail
- Enable GuardDuty
- Enable Inspector
- Keep systems patched

---

# High Availability

```
Internet
      │
      ▼
Application Load Balancer
      │
      ▼
Auto Scaling Group
      │
 ┌────┴────┐
 ▼         ▼
EC2-A    EC2-B
(AZ-1)   (AZ-2)
```

---

# Monitoring Services

| Service | Purpose |
|----------|----------|
| CloudWatch | Metrics & Alarms |
| CloudTrail | API Logging |
| AWS Config | Configuration Tracking |
| GuardDuty | Threat Detection |
| Inspector | Vulnerability Scanning |
| Systems Manager | Fleet Management |

---

# Common Linux Commands

### CPU

```bash
top
```

```bash
htop
```

---

### Memory

```bash
free -h
```

---

### Disk Usage

```bash
df -h
```

---

### Largest Directories

```bash
du -sh /*
```

---

### Processes

```bash
ps -ef
```

---

### Network Ports

```bash
ss -tulnp
```

---

### Storage Devices

```bash
lsblk
```

---

### Logs

```bash
journalctl -xe
```

---

# Frequently Used AWS CLI Commands

Configure CLI

```bash
aws configure
```

List Instances

```bash
aws ec2 describe-instances
```

Start Instance

```bash
aws ec2 start-instances --instance-ids i-xxxxxxxx
```

Stop Instance

```bash
aws ec2 stop-instances --instance-ids i-xxxxxxxx
```

Reboot Instance

```bash
aws ec2 reboot-instances --instance-ids i-xxxxxxxx
```

Terminate Instance

```bash
aws ec2 terminate-instances --instance-ids i-xxxxxxxx
```

Describe Instance Status

```bash
aws ec2 describe-instance-status
```

Create Snapshot

```bash
aws ec2 create-snapshot --volume-id vol-xxxxxxxx
```

Create AMI

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name MyAMI
```

---

# Troubleshooting Checklist

Unable to SSH?

- Instance Running?
- Public IP available?
- Security Group allows Port 22?
- NACL configured?
- Route Table correct?
- Internet Gateway attached?
- SSH service running?

Website Not Opening?

- Web server running?
- Port 80/443 open?
- Security Group configured?
- DNS correct?
- Application healthy?

High CPU?

```bash
top
```

Disk Full?

```bash
df -h
```

Memory High?

```bash
free -h
```

Storage Issues?

```bash
lsblk
```

Logs?

```bash
journalctl -xe
```

---

# Best Practices

- Use Auto Scaling Groups
- Deploy across multiple Availability Zones
- Place instances behind Load Balancers
- Use Private Subnets for application servers
- Use IAM Roles instead of Access Keys
- Encrypt EBS volumes
- Enable CloudWatch monitoring
- Enable CloudTrail logging
- Create regular AMIs and Snapshots
- Tag all resources
- Patch operating systems regularly
- Use Infrastructure as Code (Terraform/CloudFormation)

---

# Common Interview Questions

- What is EC2?
- Explain the EC2 lifecycle.
- Difference between Stop and Terminate.
- Difference between Security Group and NACL.
- Difference between EBS and Instance Store.
- What is an AMI?
- What is Auto Scaling?
- What is a Launch Template?
- Explain EC2 pricing models.
- How do you secure an EC2 instance?

---

# Production Architecture

```
Users
   │
   ▼
Route53
   │
   ▼
AWS WAF
   │
   ▼
Application Load Balancer
   │
   ▼
Auto Scaling Group
   │
 ┌────┴────┐
 ▼         ▼
EC2      EC2
 │         │
 └────┬────┘
      ▼
Amazon RDS
      │
      ▼
Amazon S3
      │
      ▼
CloudWatch
      │
      ▼
CloudTrail
```

---

# One-Minute Revision

```
EC2 = Virtual Machine

AMI = Operating System Image

Instance Type = CPU + RAM

EBS = Persistent Storage

Security Group = Stateful Firewall

NACL = Stateless Firewall

IAM Role = Secure AWS Access

ALB = Traffic Distribution

ASG = Automatic Scaling

CloudWatch = Monitoring

CloudTrail = API Logs

Snapshot = Volume Backup

Elastic IP = Static Public IP

Region = Geographic Area

Availability Zone = Isolated Data Center
```

---

# Summary

Amazon EC2 is the core compute service of AWS. Mastering EC2 means understanding instance lifecycle, networking, storage, security, monitoring, pricing, Auto Scaling, Load Balancing, troubleshooting, and production best practices. This cheat sheet provides a quick reference for interviews, certifications, and daily DevOps operations.

---

# Related Topics

- Amazon EBS
- Amazon VPC
- IAM
- Auto Scaling
- Elastic Load Balancer
- CloudWatch
- CloudTrail
- AWS Systems Manager
- AWS Backup