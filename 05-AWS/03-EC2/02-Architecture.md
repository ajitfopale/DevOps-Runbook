# EC2 Architecture

> **Document Version:** 1.0  
> **Category:** AWS EC2  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 15 Minutes

---

# Objective

After reading this document, you will understand:

- EC2 architecture
- How an EC2 instance is created
- Components involved in launching an EC2 instance
- Request flow from user to virtual machine
- Real production architecture

---

# What is EC2 Architecture?

EC2 Architecture explains how Amazon launches and manages a virtual server (EC2 Instance) using AWS infrastructure.

When you click **Launch Instance**, AWS performs many background operations before your virtual machine is ready.

---

# Basic EC2 Architecture

```text
                     User
                      │
                      ▼
             AWS Management Console
                AWS CLI / API
                      │
                      ▼
               IAM Authentication
                      │
                      ▼
               EC2 Control Plane
                      │
      ┌───────────────┼───────────────┐
      │               │               │
      ▼               ▼               ▼
     AMI        Instance Type    Key Pair
      │               │               │
      └───────────────┼───────────────┘
                      ▼
                  VPC Network
                      │
              Security Group
                      │
                 Subnet & AZ
                      │
                 EBS Volume
                      │
                      ▼
               EC2 Virtual Machine
                      │
              Public / Private IP
                      │
                      ▼
                  SSH / RDP Access
```

---

# Components of EC2 Architecture

## 1. User

The process starts when a user launches an EC2 instance.

The user can use:

- AWS Console
- AWS CLI
- SDK
- Terraform
- CloudFormation

---

## 2. IAM

IAM verifies:

- Who is launching the instance
- Whether they have permission
- Which AWS account they belong to

If permission is denied, the instance will not be created.

---

## 3. EC2 Control Plane

The EC2 service receives the request and coordinates the creation of the instance.

It communicates with other AWS services to allocate resources.

---

## 4. Amazon Machine Image (AMI)

AMI is a template containing:

- Operating System
- Packages
- Configuration
- Boot volume

Examples:

- Amazon Linux
- Ubuntu
- Red Hat Enterprise Linux
- Windows Server

---

## 5. Instance Type

Instance type defines:

- vCPUs
- RAM
- Network performance
- Storage performance

Examples:

- t2.micro
- t3.small
- m5.large
- c6i.large

---

## 6. VPC

Every EC2 instance runs inside a Virtual Private Cloud (VPC).

The VPC provides:

- Network isolation
- Routing
- Security
- IP addressing

---

## 7. Subnet

The subnet determines where the instance will be placed.

Example:

```
VPC
├── Public Subnet
└── Private Subnet
```

---

## 8. Availability Zone

Each subnet belongs to one Availability Zone.

Example:

```
Mumbai Region

ap-south-1a

ap-south-1b

ap-south-1c
```

AWS launches the instance in the selected Availability Zone.

---

## 9. Security Group

Acts as a virtual firewall.

Controls:

- SSH (22)
- HTTP (80)
- HTTPS (443)
- Custom Ports

Example:

```
SSH → Allowed

HTTP → Allowed

MySQL → Blocked
```

---

## 10. EBS Volume

When the instance starts:

AWS automatically creates the root EBS volume.

Example:

```
Amazon Linux

↓

20 GB gp3 EBS
```

Additional EBS volumes can also be attached.

---

## 11. Hypervisor

AWS runs many virtual machines on powerful physical servers.

The hypervisor:

- Creates virtual machines
- Allocates CPU
- Allocates RAM
- Manages storage
- Provides isolation

Users never access the physical server directly.

---

## 12. Public IP

If enabled:

AWS assigns a public IPv4 address.

Example:

```
54.xxx.xxx.xxx
```

---

## 13. Elastic IP (Optional)

Instead of a changing public IP,

you can assign a fixed Elastic IP.

Useful for:

- Production servers
- DNS mapping

---

## 14. SSH Connection

Linux:

```
ssh -i key.pem ec2-user@Public-IP
```

Windows:

Use RDP.

---

# EC2 Launch Workflow

```
User

↓

Console / CLI

↓

IAM

↓

EC2 Service

↓

Select AMI

↓

Select Instance Type

↓

Choose VPC

↓

Choose Subnet

↓

Create EBS

↓

Attach Security Group

↓

Launch Instance

↓

Running State
```

---

# Real Production Example

Imagine an e-commerce company.

Traffic arrives through:

```
Internet

↓

Load Balancer

↓

EC2 Instance 1

EC2 Instance 2

EC2 Instance 3

↓

RDS Database

↓

S3 Storage
```

If one EC2 instance fails,

the Load Balancer automatically sends traffic to the remaining healthy instances.

---

# Best Practices

- Use IAM Roles instead of storing credentials.
- Use Security Groups with least privilege.
- Keep instances in private subnets when possible.
- Use Auto Scaling.
- Use Load Balancers.
- Enable CloudWatch monitoring.
- Take regular AMI and EBS backups.

---

# Common Mistakes

- Opening SSH to `0.0.0.0/0` unnecessarily.
- Launching resources in the wrong Region.
- Choosing the wrong instance type.
- Forgetting to stop unused instances.
- Deleting the root EBS volume accidentally.

---

# Interview Questions

### Beginner

1. What is EC2 architecture?
2. What is an AMI?
3. What is an instance type?
4. What is a Security Group?
5. What is an EBS volume?

### Intermediate

1. Explain the EC2 launch process.
2. How does IAM interact with EC2?
3. Why does EC2 require a VPC?
4. What is the role of the hypervisor?
5. Explain the difference between a public IP and an Elastic IP.

---

# Summary

EC2 architecture consists of multiple AWS services working together to create a secure, scalable, and highly available virtual machine. Understanding how IAM, AMI, VPC, Subnets, Security Groups, EBS, and the EC2 control plane interact is essential for designing reliable cloud infrastructure.

---

# References

- AWS EC2 Documentation: https://docs.aws.amazon.com/ec2/