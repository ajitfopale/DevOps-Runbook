# Amazon VPC (Virtual Private Cloud)

## What is VPC?

**VPC** stands for **Virtual Private Cloud**.

Amazon VPC is a service that allows you to create a **logically isolated virtual network** inside AWS where you can launch and manage your AWS resources securely.

In simple words:

> **A VPC is your own private network inside the AWS Cloud.**

You can launch resources like:

- EC2 Instances
- RDS Databases
- Elastic Load Balancers
- EFS
- Lambda (inside VPC)
- ECS Tasks
- Other AWS services

inside your own private network.

---

## Definition

> **Amazon VPC is a logically isolated virtual network that allows you to securely launch AWS resources in the AWS Cloud.**

---

## Why Do We Need a VPC?

VPC provides:

- Isolation from other AWS customers
- Secure networking
- Control over IP addresses
- Public and Private Networks
- Internet Connectivity
- Route Management
- Firewall using Security Groups and NACLs

---

## Key Features of VPC

- Logically isolated virtual network
- Supports both Public and Private Subnets
- Custom IP Address Range (CIDR Block)
- Supports IPv4 and IPv6
- Route Tables for traffic management
- Internet Gateway for Internet access
- NAT Gateway for Private Subnets
- Security Groups (Instance Level Firewall)
- Network ACLs (Subnet Level Firewall)

---

## Important Point

A **VPC spans across multiple Availability Zones (AZs)** within the same AWS Region.

This means:

- One VPC can contain multiple subnets.
- Those subnets can be created in different Availability Zones.
- Resources in different AZs can communicate within the same VPC (subject to routing and security rules).

---

## Real-World Example

Imagine a company has:

- Web Servers
- Application Servers
- Database Servers

They create one VPC.

Inside that VPC:

- Public Subnet → Web Servers
- Private Subnet → Application Servers
- Private Subnet → Database Servers

This keeps the infrastructure secure and organized.

---

## Basic Architecture

```text
                 AWS Region
                      │
          ┌─────────────────────┐
          │        VPC          │
          │ 10.0.0.0/16         │
          └─────────┬───────────┘
                    │
      ┌─────────────┴─────────────┐
      │                           │
┌───────────────┐         ┌───────────────┐
│ Public Subnet │         │ Private Subnet│
│ 10.0.1.0/24   │         │ 10.0.2.0/24   │
└──────┬────────┘         └──────┬────────┘
       │                         │
   Internet                 Database/App
    Access                    Servers
```

---

## Common AWS Resources Inside a VPC

- EC2
- RDS
- EFS
- Elastic Load Balancer (ALB/NLB)
- ECS
- Lambda (VPC-enabled)
- NAT Gateway
- Internet Gateway

---

## Interview Questions

### 1. What is VPC?

A Virtual Private Cloud is a logically isolated virtual network in AWS where you can securely launch AWS resources.

---

### 2. Is VPC Region Specific or AZ Specific?

**Answer:** **Region Specific**

A VPC belongs to one AWS Region but can span multiple Availability Zones.

---

### 3. Can one VPC have multiple Availability Zones?

**Answer:** Yes.

A single VPC can span all Availability Zones within the same Region.

---

### 4. Can one EC2 communicate with another EC2 inside the same VPC?

**Answer:** Yes, if routing and security rules allow it.

---

## Summary

- **VPC = Virtual Private Cloud**
- A logically isolated virtual network in AWS
- Used to securely host AWS resources
- **Region-specific** service
- Spans multiple **Availability Zones (AZs)**
- Supports Public and Private Subnets
- Provides secure networking with Security Groups, NACLs, Route Tables, and Gateways