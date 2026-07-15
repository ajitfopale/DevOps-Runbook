# What is AWS?

> **Document Version:** 1.0  
> **Category:** AWS Fundamentals  
> **Difficulty:** Beginner  
> **Estimated Reading Time:** 15–20 Minutes

---

# Objective

After reading this document, you will understand:

- What AWS is
- Why AWS was created
- What problems AWS solves
- How AWS works
- Where AWS is used
- Basic AWS concepts
- Real-world examples
- Important interview points

---

# Introduction

Imagine you want to create a website.

To run that website, you need:

- A server
- Storage
- Database
- Network
- Security
- Backup
- Monitoring

Earlier, companies had to purchase all this hardware themselves.

Example:

A company buys:

- Physical servers
- Hard disks
- Routers
- Firewalls
- Networking equipment
- UPS
- Air conditioning
- Rack cabinets

This requires:

- Huge investment
- Dedicated office space
- IT engineers
- Maintenance
- Electricity
- Cooling

This model is called **Traditional On-Premises Infrastructure**.

---

# What is AWS?

**AWS (Amazon Web Services)** is a cloud computing platform provided by Amazon.

It allows individuals and organizations to rent IT resources over the internet instead of purchasing physical hardware.

With AWS, you can create and manage:

- Virtual servers
- Storage
- Databases
- Networking
- Security
- Monitoring
- Artificial Intelligence services
- Machine Learning services
- Containers
- Serverless applications

AWS follows a **Pay-As-You-Go** pricing model.

You pay only for the resources you use.

---

# Simple Definition

AWS is a collection of cloud services that allows you to build, deploy, manage, and scale applications without owning physical infrastructure.

---

# Real-Life Example

Think of electricity.

You don't build your own power plant.

Instead, you pay the electricity company only for the electricity you consume.

AWS works similarly.

Instead of buying servers, you rent them whenever you need them.

---

# Why Was AWS Created?

Before cloud computing:

- Companies purchased expensive hardware.
- Setting up infrastructure took weeks or months.
- Scaling applications required buying more servers.
- Hardware failures caused downtime.
- Infrastructure maintenance was expensive.

Amazon faced these challenges while running its own business.

They built tools to solve these problems internally and later made them available to everyone as AWS.

AWS officially launched in **2006**.

---

# How AWS Works

AWS owns large data centers around the world.

Inside these data centers are thousands of physical servers.

When you launch an EC2 instance, AWS allocates resources from these physical servers and presents them to you as a virtual machine.

You interact with AWS using:

- AWS Management Console
- AWS CLI
- SDKs
- APIs

---

# What Can You Build Using AWS?

Examples include:

- Personal websites
- Company websites
- E-commerce platforms
- Banking applications
- Mobile app backends
- Video streaming platforms
- Online gaming services
- AI and Machine Learning applications
- Data analytics platforms
- Backup and disaster recovery solutions

---

# Popular AWS Services

| Service | Purpose |
|----------|---------|
| EC2 | Virtual Servers |
| S3 | Object Storage |
| EBS | Block Storage |
| EFS | Shared File Storage |
| IAM | Identity and Access Management |
| VPC | Virtual Network |
| RDS | Managed Databases |
| Route 53 | DNS Service |
| CloudWatch | Monitoring |
| CloudTrail | Audit Logs |
| Lambda | Serverless Computing |
| EKS | Kubernetes Service |
| ECS | Container Service |

---

# Advantages of AWS

- No upfront hardware cost
- Pay only for what you use
- Highly scalable
- High availability
- Global infrastructure
- Strong security features
- Automatic backups
- Managed services
- Fast deployment
- Reliable infrastructure

---

# Limitations

- Internet connection is required.
- Costs can increase if resources are not managed properly.
- Many services require time to learn.
- Vendor-specific knowledge is needed.

---

# Real Production Example

Imagine an online shopping website during a festival sale.

Normally:

- 10,000 users visit every day.

During the sale:

- 10,00,000 users visit in one day.

Without cloud computing, the website may crash.

With AWS:

- More servers can be added automatically.
- Load Balancers distribute traffic.
- Auto Scaling adjusts capacity based on demand.
- After the sale, extra servers are removed automatically.

This helps reduce costs while maintaining performance.

---

# Where AWS Is Used

AWS is used across many industries, including:

- Banking
- Healthcare
- Retail
- E-commerce
- Education
- Government
- Gaming
- Media
- Telecommunications
- Startups

---

# Key Terms

| Term | Meaning |
|------|---------|
| Cloud | IT resources delivered over the internet |
| Region | Geographic area where AWS operates |
| Availability Zone | One or more isolated data centers within a Region |
| EC2 | Virtual Machine |
| S3 | Object Storage |
| IAM | Identity and Access Management |

---

# Best Practices

- Enable Multi-Factor Authentication (MFA).
- Follow the principle of least privilege.
- Monitor resource usage.
- Delete unused resources.
- Tag resources consistently.
- Enable logging and monitoring.

---

# Common Beginner Mistakes

- Leaving EC2 instances running unnecessarily.
- Creating resources in the wrong Region.
- Using the root account for daily work.
- Forgetting to delete unused resources.
- Opening Security Groups to the entire internet (`0.0.0.0/0`) without understanding the risks.

---

# Interview Questions

### Beginner

1. What is AWS?
2. What does AWS stand for?
3. Why do companies use AWS?
4. What is cloud computing?
5. What is the Pay-As-You-Go model?

### Intermediate

1. Explain the advantages of AWS over on-premises infrastructure.
2. How does AWS achieve scalability?
3. Name five AWS services and their uses.
4. What are Regions and Availability Zones?
5. How do you access AWS services?

---

# Summary

AWS is the world's leading cloud computing platform. It provides a wide range of on-demand services, allowing organizations to build and scale applications without investing in physical infrastructure. Its flexibility, global reach, and pay-as-you-go pricing make it a popular choice for businesses of all sizes.

---

# References

- AWS Official Documentation: https://docs.aws.amazon.com/
- AWS Training and Certification: https://aws.amazon.com/training/