# Amazon VPC Architecture

## What is VPC Architecture?

Amazon VPC Architecture is the design of how AWS networking components work together to provide secure communication between resources.

A VPC contains different networking components such as:

- CIDR Block
- Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Security Groups
- Network ACLs
- EC2 Instances

These components work together to control network traffic and secure your AWS infrastructure.

---

# Basic VPC Architecture

```text
                           AWS Region
                                │
                     ┌────────────────────┐
                     │        VPC         │
                     │    10.0.0.0/16     │
                     └─────────┬──────────┘
                               │
             ┌─────────────────┴─────────────────┐
             │                                   │
      Public Subnet                       Private Subnet
      10.0.1.0/24                         10.0.2.0/24
             │                                   │
             │                                   │
      ┌─────────────┐                    ┌─────────────┐
      │    EC2      │                    │     EC2     │
      │ Web Server  │                    │ App Server  │
      └──────┬──────┘                    └──────┬──────┘
             │                                   │
             │                                   │
      Internet Gateway                    NAT Gateway
             │
         Internet
```

---

# Components of VPC Architecture

## 1. VPC

- A logically isolated virtual network.
- Created within a single AWS Region.
- Can span multiple Availability Zones.

Example:

```
10.0.0.0/16
```

---

## 2. CIDR Block

CIDR (Classless Inter-Domain Routing) defines the IP address range of the VPC.

Example:

```
10.0.0.0/16
```

This provides **65,536 IP addresses**.

---

## 3. Subnet

A subnet is a smaller network inside a VPC.

There are two types:

- Public Subnet
- Private Subnet

Example:

```
Public Subnet
10.0.1.0/24

Private Subnet
10.0.2.0/24
```

---

## 4. Internet Gateway (IGW)

Internet Gateway allows resources in a Public Subnet to communicate with the Internet.

Without an Internet Gateway:

- No Internet access
- Cannot SSH using Public IP
- Cannot browse websites

---

## 5. NAT Gateway

NAT Gateway provides Internet access to instances in a Private Subnet without exposing them to incoming Internet traffic.

Used for:

- Software updates
- Package installation
- Downloading dependencies

---

## 6. Route Table

A Route Table controls where network traffic should go.

Example:

| Destination | Target |
|------------|--------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

---

## 7. Security Group

Security Group acts as a virtual firewall for EC2 instances.

Controls:

- Inbound Traffic
- Outbound Traffic

Example:

| Port | Purpose |
|------|----------|
|22|SSH|
|80|HTTP|
|443|HTTPS|

---

## 8. Network ACL (NACL)

Network ACL acts as a firewall at the Subnet level.

It controls traffic entering and leaving an entire subnet.

Unlike Security Groups:

- Supports Allow and Deny Rules.
- Stateless firewall.

---

## 9. EC2 Instance

EC2 instances are virtual servers launched inside a subnet.

Examples:

- Web Server
- Application Server
- Database Server

---

# Public VPC Architecture

```text
Internet
    │
    │
Internet Gateway
    │
    │
Public Route Table
    │
Public Subnet
    │
EC2 Instance
```

The EC2 instance has:

- Public IP
- Internet Access

---

# Private VPC Architecture

```text
Internet
    │
Internet Gateway
    │
Public Subnet
    │
NAT Gateway
    │
Private Route Table
    │
Private Subnet
    │
EC2 Instance
```

The EC2 instance:

- Has no Public IP
- Cannot receive incoming Internet traffic
- Can access the Internet through the NAT Gateway

---

# Communication Flow

## Public EC2

```
Internet
↓

Internet Gateway
↓

Route Table
↓

Public Subnet
↓

EC2 Instance
```

---

## Private EC2

```
Private EC2
↓

Private Route Table
↓

NAT Gateway
↓

Internet Gateway
↓

Internet
```

---

# Real-World Example

A company hosts an e-commerce application.

### Public Subnet

- Web Server
- Load Balancer
- Bastion Host

### Private Subnet

- Application Server
- Database Server
- Cache Server

Benefits:

- Secure architecture
- Better performance
- High availability
- Controlled Internet access

---

# Key Points

- VPC is Region-specific.
- A VPC can span multiple Availability Zones.
- Public Subnets connect to the Internet using an Internet Gateway.
- Private Subnets access the Internet using a NAT Gateway.
- Route Tables define traffic paths.
- Security Groups protect instances.
- Network ACLs protect subnets.

---

# Interview Questions

### What is VPC Architecture?

VPC Architecture is the design of AWS networking components such as VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway, Security Groups, and Network ACLs that work together to provide secure communication.

---

### What is the difference between Public and Private Subnets?

| Public Subnet | Private Subnet |
|--------------|----------------|
| Has Internet Access | No Direct Internet Access |
| Uses Internet Gateway | Uses NAT Gateway |
| Public IP Allowed | Public IP Not Required |

---

### Which component provides Internet access to a Public Subnet?

**Internet Gateway (IGW)**

---

### Which component provides Internet access to a Private Subnet?

**NAT Gateway**

---

# Summary

- VPC is the foundation of AWS networking.
- A VPC contains multiple networking components.
- Public resources use an Internet Gateway.
- Private resources use a NAT Gateway.
- Security Groups and NACLs provide network security.
- Route Tables control network traffic.
- Together, these components create a secure and scalable AWS network.