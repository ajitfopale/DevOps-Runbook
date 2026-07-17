# Amazon VPC Components

## What are VPC Components?

VPC Components are the networking resources that work together to build a secure and scalable network inside AWS.

Each component has a specific role, such as:

- Creating a network
- Dividing the network into subnets
- Connecting to the Internet
- Controlling network traffic
- Securing resources

---

# List of VPC Components

1. VPC
2. CIDR Block
3. Subnet
4. Route Table
5. Internet Gateway (IGW)
6. NAT Gateway
7. Security Group (SG)
8. Network ACL (NACL)
9. Elastic IP (EIP)
10. DHCP Option Set
11. VPC Endpoints
12. VPC Peering
13. Transit Gateway

---

# 1. VPC (Virtual Private Cloud)

A VPC is a logically isolated virtual network where you launch AWS resources.

### Example

```
VPC CIDR: 10.0.0.0/16
```

### Purpose

- Creates your private network
- Isolates AWS resources
- Supports multiple Availability Zones

---

# 2. CIDR Block

CIDR (Classless Inter-Domain Routing) defines the IP address range of a VPC or Subnet.

### Example

```
VPC CIDR

10.0.0.0/16
```

Subnet

```
10.0.1.0/24
```

### Purpose

- Provides IP addresses
- Helps divide the network into subnets

---

# 3. Subnet

A subnet is a smaller network inside a VPC.

### Types

- Public Subnet
- Private Subnet

### Purpose

- Organizes resources
- Improves security
- Separates public and private workloads

---

# 4. Route Table

A Route Table contains rules that decide where network traffic should go.

### Example

| Destination | Target |
|-------------|--------|
|10.0.0.0/16|Local|
|0.0.0.0/0|Internet Gateway|

### Purpose

- Directs network traffic
- Connects resources to internal or external networks

---

# 5. Internet Gateway (IGW)

An Internet Gateway allows communication between a VPC and the Internet.

### Purpose

- Provides Internet access
- Enables inbound and outbound traffic
- Used by Public Subnets

---

# 6. NAT Gateway

A NAT Gateway allows instances in Private Subnets to access the Internet without exposing them to incoming Internet traffic.

### Purpose

- Software updates
- Download packages
- Secure Internet access for private resources

---

# 7. Security Group (SG)

A Security Group acts as a virtual firewall for EC2 instances.

It controls:

- Inbound Traffic
- Outbound Traffic

### Example

| Port | Service |
|------|----------|
|22|SSH|
|80|HTTP|
|443|HTTPS|

### Purpose

- Protect EC2 instances
- Allow only required traffic

---

# 8. Network ACL (NACL)

A Network ACL is a firewall that protects an entire subnet.

Unlike Security Groups, it supports both:

- Allow Rules
- Deny Rules

### Purpose

- Controls subnet-level traffic
- Adds an extra layer of security

---

# 9. Elastic IP (EIP)

An Elastic IP is a static public IPv4 address.

It can be attached to:

- EC2 Instance
- NAT Gateway

### Purpose

- Provides a fixed public IP
- Prevents IP changes after reboot or stop/start

---

# 10. DHCP Option Set

DHCP Option Set provides network configuration automatically.

It includes:

- DNS Server
- Domain Name
- NTP Server (optional)

### Purpose

- Automatically configures networking for EC2 instances

---

# 11. VPC Endpoint

A VPC Endpoint allows private communication between a VPC and supported AWS services without using the Internet.

### Example Services

- Amazon S3
- DynamoDB

### Purpose

- Secure private access
- No Internet Gateway required

---

# 12. VPC Peering

VPC Peering connects two VPCs privately.

Resources in both VPCs can communicate using private IP addresses.

### Purpose

- Connect multiple VPCs
- Share resources securely

---

# 13. Transit Gateway

Transit Gateway connects multiple VPCs and on-premises networks through a single central gateway.

### Purpose

- Simplifies network management
- Reduces the number of VPC Peering connections

---

# VPC Components Relationship

```text
                     AWS Region
                          │
                    ┌─────────────┐
                    │     VPC     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
       CIDR Block      Route Table      Internet Gateway
          │                                 │
      ┌───┴────┐                            │
      │        │                            │
 Public      Private                    Internet
 Subnet      Subnet
    │            │
    │        NAT Gateway
    │            │
   EC2          EC2

Security Group → Protects EC2

Network ACL → Protects Subnets

Elastic IP → Used with EC2/NAT Gateway

VPC Endpoint → Private access to AWS Services

VPC Peering → Connects two VPCs

Transit Gateway → Connects multiple VPCs
```

---

# Quick Summary

| Component | Purpose |
|-----------|---------|
| VPC | Creates a private virtual network |
| CIDR Block | Defines IP address range |
| Subnet | Divides the VPC into smaller networks |
| Route Table | Controls traffic routing |
| Internet Gateway | Provides Internet access |
| NAT Gateway | Internet access for Private Subnets |
| Security Group | Instance-level firewall |
| Network ACL | Subnet-level firewall |
| Elastic IP | Static public IP address |
| DHCP Option Set | Automatic network configuration |
| VPC Endpoint | Private access to AWS services |
| VPC Peering | Private connection between two VPCs |
| Transit Gateway | Central hub for connecting multiple VPCs |

---

# Interview Questions

### 1. What are the main components of a VPC?

- VPC
- CIDR Block
- Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Security Groups
- Network ACLs
- Elastic IP
- VPC Endpoints
- VPC Peering
- Transit Gateway

---

### 2. Which VPC component provides Internet access?

**Internet Gateway (IGW)**

---

### 3. Which component acts as an instance-level firewall?

**Security Group**

---

### 4. Which component acts as a subnet-level firewall?

**Network ACL (NACL)**

---

### 5. Which component provides Internet access to Private Subnets?

**NAT Gateway**