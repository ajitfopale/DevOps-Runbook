# Amazon EC2 - Hands-On Lab

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 45 Minutes

---

# Objective

In this lab, you will learn how to:

- Launch an EC2 instance
- Connect using SSH
- Install Apache Web Server
- Host a website
- Change Apache port
- Configure Security Groups
- Attach an EBS Volume
- Format and Mount EBS
- Configure Persistent Mounting
- Verify everything works
- Clean up AWS resources

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- EC2 Key Pair (.pem)
- Basic Linux knowledge
- AWS Console access
- Internet connection

---

# Lab Architecture

```
Internet

↓

Security Group

↓

EC2 Instance

↓

Amazon Linux 2023

↓

Apache Web Server

↓

Amazon EBS Volume

↓

Website Files
```

---

# Lab 1 - Launch an EC2 Instance

## Step 1

Go to

```
AWS Console

↓

EC2

↓

Launch Instance
```

---

## Step 2

Enter

```
Name

↓

DevOps-WebServer
```

---

## Step 3

Choose AMI

```
Amazon Linux 2023
```

---

## Step 4

Choose Instance Type

```
t2.micro

or

t3.micro
```

---

## Step 5

Select Key Pair

Example

```
my-key.pem
```

---

## Step 6

Configure Network

Choose

- Default VPC
- Public Subnet
- Auto Assign Public IP = Enabled

---

## Step 7

Configure Security Group

Allow

| Port | Protocol |
|-------|----------|
| 22 | SSH |
| 80 | HTTP |

Launch Instance.

---

# Lab 2 - Connect Using SSH

Example

```bash
chmod 400 my-key.pem

ssh -i my-key.pem ec2-user@Public-IP
```

Verify

```bash
hostname

whoami
```

Expected Output

```
ec2-user
```

---

# Lab 3 - Update Server

```bash
sudo dnf update -y
```

---

# Lab 4 - Install Apache

```bash
sudo dnf install httpd -y
```

Start Service

```bash
sudo systemctl start httpd
```

Enable at Boot

```bash
sudo systemctl enable httpd
```

Check Status

```bash
systemctl status httpd
```

---

# Lab 5 - Verify Website

Create HTML page

```bash
echo "<h1>Welcome to EC2</h1>" | sudo tee /var/www/html/index.html
```

Open Browser

```
http://Public-IP
```

Expected

```
Welcome to EC2
```

---

# Lab 6 - Change Apache Port

Edit Configuration

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Change

```
Listen 80
```

to

```
Listen 81
```

Restart Apache

```bash
sudo systemctl restart httpd
```

Verify

```bash
ss -tlnp | grep 81
```

---

# Lab 7 - Update Security Group

Add

| Port | Protocol |
|-------|----------|
| 81 | TCP |

Open Browser

```
http://Public-IP:81
```

Website should load successfully.

---

# Lab 8 - Create EBS Volume

Go to

```
EC2

↓

Volumes

↓

Create Volume
```

Configuration

```
Size

10 GB

Type

gp3

Availability Zone

Same as EC2
```

Create Volume.

---

# Lab 9 - Attach EBS Volume

Choose

```
Actions

↓

Attach Volume
```

Device Name

```
/dev/xvdf
```

---

# Lab 10 - Verify Disk

```bash
lsblk
```

Example

```
xvda

xvdf
```

---

# Lab 11 - Create Filesystem

```bash
sudo mkfs -t xfs /dev/xvdf
```

---

# Lab 12 - Create Mount Point

```bash
sudo mkdir /data
```

---

# Lab 13 - Mount Volume

```bash
sudo mount /dev/xvdf /data
```

Verify

```bash
df -h
```

---

# Lab 14 - Persistent Mount

Find UUID

```bash
sudo blkid
```

Example

```
UUID=xxxxxxxx
```

Edit

```bash
sudo vi /etc/fstab
```

Add

```
UUID=xxxxxxxx /data xfs defaults,nofail 0 2
```

Test

```bash
sudo umount /data

sudo mount -a
```

Verify

```bash
df -h
```

---

# Lab 15 - Verify Website Files

```bash
cd /var/www/html

ls
```

---

# Useful Commands

```bash
hostname

pwd

lsblk

df -h

free -m

uptime

top

systemctl status httpd

curl localhost

ip addr

cat /etc/fstab

mount

umount
```

---

# Troubleshooting

## SSH Not Working

Check

- Public IP
- Security Group
- Key Pair
- SSH Service

---

## Website Not Opening

Check

```bash
systemctl status httpd
```

Check

```bash
ss -tlnp
```

Verify Security Group

---

## EBS Not Mounting

Check

```bash
lsblk

blkid

mount -a
```

Verify `/etc/fstab` entry.

---

# Cleanup

Delete

- EC2 Instance
- EBS Volume (if no longer needed)
- Elastic IP (if allocated)
- Security Group (if created only for this lab)

---

# Best Practices

- Use IAM Roles instead of access keys.
- Restrict SSH access to trusted IP addresses.
- Keep the operating system updated.
- Use gp3 for new EBS volumes.
- Tag all AWS resources.
- Take EBS snapshots before major changes.

---

# Interview Questions

1. How do you launch an EC2 instance?
2. How do you connect to Linux EC2?
3. What is a Key Pair?
4. Why is the Security Group important?
5. How do you attach an EBS volume?
6. How do you make an EBS mount persistent?
7. How do you verify Apache is running?
8. What happens if `/etc/fstab` is configured incorrectly?
9. How do you troubleshoot an EC2 instance that is unreachable?
10. Which Linux commands do you commonly use on EC2?

---

# Summary

This lab demonstrated the complete lifecycle of working with an Amazon EC2 instance—from launching and connecting to the server, installing and configuring Apache, modifying the web server port, attaching and mounting an EBS volume, configuring persistent storage, verifying services, troubleshooting common issues, and cleaning up resources. These are practical skills commonly used by DevOps Engineers, Cloud Engineers, and Linux System Administrators.

---

# Related Topics

- Amazon EBS
- AMI
- Security Groups
- IAM
- CloudWatch
- Auto Scaling
- Elastic Load Balancer

---

# References

- AWS EC2 User Guide: https://docs.aws.amazon.com/ec2/
- AWS Amazon Linux Documentation: https://docs.aws.amazon.com/linux/