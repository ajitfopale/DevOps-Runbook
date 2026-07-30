# Amazon EC2 Troubleshooting Guide

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Intermediate to Advanced  
> **Estimated Reading Time:** 60 Minutes

---

# Objective

After reading this document, you will learn:

- Common EC2 production issues
- How to troubleshoot EC2 systematically
- AWS services used during troubleshooting
- Linux commands for troubleshooting
- Network troubleshooting
- Storage troubleshooting
- Performance troubleshooting
- Real-world production scenarios

---

# Troubleshooting Methodology

Always follow a structured approach.

```
Problem

↓

Identify Symptoms

↓

Collect Logs

↓

Check AWS Resources

↓

Check Operating System

↓

Identify Root Cause

↓

Fix

↓

Validate

↓

Document
```

Never start making changes before understanding the root cause.

---

# EC2 Health Checks

AWS automatically performs three health checks.

## 1. System Status Check

Checks AWS infrastructure.

Examples

- Hardware failure
- Power failure
- Network issue
- Hypervisor issue

AWS is responsible.

---

## 2. Instance Status Check

Checks the operating system.

Examples

- Kernel panic
- Boot failure
- Corrupted file system
- Memory issue

Customer is responsible.

---

## 3. Attached EBS Status Check

Checks attached EBS volume health.

---

# Problem 1: Unable to SSH into EC2

Symptoms

```
ssh: connect to host xx.xx.xx.xx port 22: Connection timed out
```

Possible Causes

- EC2 stopped
- Wrong Public IP
- Port 22 blocked
- Security Group issue
- NACL issue
- Route Table issue
- Internet Gateway missing
- SSH service stopped

Checklist

- Is the instance running?
- Does it have a Public IP or Elastic IP?
- Is Security Group allowing TCP 22?
- Is NACL allowing inbound and outbound traffic?
- Is the route table configured correctly?
- Is the Internet Gateway attached?
- Is SSH service running?

Commands

```bash
systemctl status sshd
```

```bash
sudo systemctl restart sshd
```

---

# Problem 2: Permission Denied (publickey)

Example

```
Permission denied (publickey)
```

Possible Causes

- Wrong private key
- Incorrect username
- Wrong file permissions

Fix

Correct permission

```bash
chmod 400 my-key.pem
```

Correct usernames

Amazon Linux

```
ec2-user
```

Ubuntu

```
ubuntu
```

RHEL

```
ec2-user
```

CentOS

```
centos
```

---

# Problem 3: Website Not Opening

Checklist

- Apache/Nginx running?
- Port 80 open?
- Security Group allows HTTP?
- Firewall enabled?
- DNS correct?

Commands

Apache

```bash
sudo systemctl status httpd
```

Nginx

```bash
sudo systemctl status nginx
```

Check listening ports

```bash
ss -tulnp
```

---

# Problem 4: Instance Not Reachable

Possible Causes

- Boot failure
- OS corruption
- Kernel panic
- File system corruption

Check

AWS Console

↓

EC2

↓

Status Checks

↓

System Log

---

# Problem 5: CPU Utilization is High

Check

CloudWatch

or

```bash
top
```

```bash
htop
```

```bash
vmstat
```

Possible Causes

- Infinite loop
- Heavy application
- Traffic spike
- Malware
- Large batch job

Solutions

- Scale vertically
- Auto Scaling
- Optimize application
- Restart failed process

---

# Problem 6: Memory Usage High

Commands

```bash
free -h
```

```bash
top
```

```bash
ps aux --sort=-%mem
```

Possible Causes

- Memory leak
- Too many processes
- Large cache

---

# Problem 7: Disk Full

Check

```bash
df -h
```

Largest directories

```bash
du -sh /*
```

Largest files

```bash
find / -type f -size +500M
```

Solutions

- Delete old logs
- Expand EBS volume
- Clean temporary files

---

# Problem 8: EBS Volume Not Mounting

Check

```bash
lsblk
```

Check filesystem

```bash
blkid
```

Mount manually

```bash
mount /dev/nvme1n1p1 /data
```

Check

```
/etc/fstab
```

Common Error

```
wrong fs type

bad superblock
```

Possible Causes

- Wrong device
- Incorrect filesystem
- Corrupted filesystem
- Incorrect fstab entry

---

# Problem 9: Internet Not Working

Checklist

- Public IP available?
- Internet Gateway attached?
- Route Table correct?
- Security Group correct?
- NACL correct?

Commands

```bash
ping 8.8.8.8
```

```bash
ping google.com
```

```bash
curl ifconfig.me
```

---

# Problem 10: DNS Resolution Failure

Test

```bash
nslookup google.com
```

```bash
dig google.com
```

Check

```
/etc/resolv.conf
```

---

# Problem 11: Application Slow

Investigate

- CPU
- Memory
- Disk IOPS
- Database
- Network

AWS Tools

- CloudWatch
- X-Ray
- Performance Insights (RDS)

---

# Problem 12: Instance Boot Failure

Possible Causes

- Corrupted kernel
- Broken fstab
- Missing root volume
- File system corruption

Recovery

- Detach root volume
- Attach to rescue instance
- Repair
- Reattach

---

# Problem 13: Lost SSH Key Pair

AWS cannot recover private keys.

Recovery

- Stop instance
- Detach root volume
- Attach to another EC2
- Add new public key to `authorized_keys`
- Reattach volume
- Start instance

Or use AWS Systems Manager Session Manager if already configured.

---

# Problem 14: Security Group Misconfiguration

Symptoms

- Website inaccessible
- SSH blocked
- Application timeout

Check

Inbound Rules

Outbound Rules

Correct ports

- 22
- 80
- 443

---

# Problem 15: Auto Scaling Not Launching Instances

Verify

- Launch Template
- AMI exists
- IAM Role
- Instance limits
- Scaling policy
- Health checks

---

# Problem 16: Load Balancer Health Check Failed

Checklist

- Application running?
- Correct health check path?
- Security Group allows ALB traffic?
- Target group registration?
- Correct port?

---

# Useful Linux Commands

CPU

```bash
top
```

Memory

```bash
free -h
```

Disk

```bash
df -h
```

Processes

```bash
ps -ef
```

Ports

```bash
ss -tulnp
```

Logs

```bash
journalctl -xe
```

Disk Usage

```bash
du -sh /*
```

Storage

```bash
lsblk
```

---

# AWS Services Used for Troubleshooting

- CloudWatch
- CloudTrail
- Systems Manager
- EC2 Console
- VPC Flow Logs
- AWS Config
- AWS Health Dashboard

---

# Troubleshooting Flowchart

```
Issue

↓

Instance Running?

↓

Status Checks Passed?

↓

SSH Working?

↓

Application Running?

↓

Network OK?

↓

Storage OK?

↓

Logs

↓

Root Cause

↓

Fix

↓

Validation
```

---

# Production Best Practices

- Enable CloudWatch monitoring
- Configure CloudTrail
- Enable Systems Manager
- Take regular snapshots
- Use Auto Scaling
- Configure Load Balancer health checks
- Maintain runbooks
- Monitor alarms proactively
- Keep operating systems patched

---

# Common Mistakes

❌ Restarting instances without investigation

❌ Ignoring CloudWatch metrics

❌ Not checking Security Groups

❌ Editing `/etc/fstab` without testing

❌ Deleting logs before analysis

❌ Not taking backups before changes

---

# Interview Questions

## Beginner

1. What are EC2 status checks?
2. Why can't you SSH into an EC2 instance?
3. How do you check disk usage?
4. How do you troubleshoot a website not opening?
5. Which command shows mounted disks?

## Intermediate

1. Explain Security Group troubleshooting.
2. How do you troubleshoot high CPU usage?
3. How do you recover from a lost key pair?
4. Explain how to troubleshoot EBS mounting issues.
5. How do you investigate an unhealthy EC2 instance?

## Advanced

1. Walk through troubleshooting an EC2 instance that is unreachable.
2. How would you diagnose intermittent application slowness?
3. Explain the process of repairing a corrupted root volume.
4. How would you troubleshoot an Auto Scaling Group that isn't launching instances?
5. Design a troubleshooting workflow for production EC2 environments.

---

# Quick Revision

```
SSH Issues
↓

Security Groups
↓

Status Checks
↓

CloudWatch
↓

Disk

↓

Memory

↓

CPU

↓

Logs

↓

Network

↓

EBS

↓

Root Cause

↓

Fix
```

---

# Summary

Troubleshooting Amazon EC2 requires a structured, methodical approach. By combining AWS tools such as CloudWatch, CloudTrail, Systems Manager, and VPC Flow Logs with Linux utilities like `top`, `free`, `df`, `journalctl`, and `ss`, engineers can quickly identify root causes and restore production services. Always validate changes, document findings, and implement preventive measures to reduce future incidents.

---

# Related Topics

- EC2 Best Practices
- EC2 Security
- Amazon EBS
- VPC
- CloudWatch
- CloudTrail
- Systems Manager
- Auto Scaling
- Elastic Load Balancer

---

# References

- AWS EC2 User Guide
- AWS Troubleshooting Documentation
- AWS Well-Architected Framework – Operational Excellence Pillar
- Amazon Linux Documentation