# Amazon EC2 Pricing

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Beginner to Intermediate  
> **Estimated Reading Time:** 30 Minutes

---

# Objective

After reading this document, you will understand:

- How Amazon EC2 pricing works
- Different EC2 purchasing options
- Billing concepts
- Which pricing model to choose
- Cost optimization strategies
- Real production examples
- Interview questions

---

# Introduction

Amazon EC2 follows a **pay-as-you-go** pricing model.

You pay only for the compute resources you use.

Unlike traditional data centers, you don't need to purchase physical servers in advance.

AWS provides multiple pricing models to optimize cost based on workload requirements.

---

# Factors Affecting EC2 Pricing

The cost of an EC2 instance depends on several factors:

- Instance Type
- Region
- Operating System
- Purchasing Option
- Storage (EBS)
- Data Transfer
- Elastic IP usage
- Monitoring options

Example:

```
EC2 Cost

↓

Instance Type

↓

Region

↓

Operating System

↓

Storage

↓

Network Usage
```

---

# EC2 Purchasing Options

AWS offers five primary purchasing models:

```
Amazon EC2 Pricing

│

├── On-Demand

├── Reserved Instances

├── Savings Plans

├── Spot Instances

└── Dedicated Hosts
```

---

# 1. On-Demand Instances

On-Demand is the default pricing model.

Features

- No long-term commitment
- Pay only while the instance runs
- Easy to start and stop

Best For

- Learning AWS
- Development
- Testing
- Short-term projects
- Unpredictable workloads

Advantages

- Flexible
- No upfront payment
- Easy to scale

Disadvantages

- Highest hourly cost

Example

```
Need Server Today

↓

Launch EC2

↓

Pay Per Hour (or per second for supported instances)
```

---

# 2. Reserved Instances (RI)

Reserved Instances provide discounts in exchange for a commitment.

Commitment Options

- 1 Year
- 3 Years

Benefits

- Lower cost than On-Demand
- Predictable pricing

Best For

- Production servers
- Databases
- Applications running continuously

Advantages

- Significant savings
- Stable pricing

Disadvantages

- Long-term commitment

---

# 3. Savings Plans

Savings Plans offer flexible pricing discounts.

Types

- Compute Savings Plans
- EC2 Instance Savings Plans

Benefits

- Similar savings to Reserved Instances
- More flexibility

Best For

- Long-running workloads
- Organizations with changing infrastructure

---

# 4. Spot Instances

Spot Instances use unused AWS capacity.

Features

- Very low cost
- AWS can interrupt the instance with short notice

Best For

- Batch processing
- CI/CD pipelines
- Big Data
- Testing
- Fault-tolerant applications

Advantages

- Lowest price
- Excellent for non-critical workloads

Disadvantages

- Can be interrupted

---

# 5. Dedicated Hosts

Dedicated Hosts provide a physical server dedicated to one customer.

Best For

- Compliance requirements
- Software licensing
- Regulatory environments

Advantages

- Dedicated hardware
- License compliance

Disadvantages

- Most expensive option

---

# Pricing Comparison

| Pricing Model | Cost | Flexibility | Best For |
|---------------|------|-------------|----------|
| On-Demand | High | High | Short-term workloads |
| Reserved Instance | Low | Low | Long-running production |
| Savings Plans | Low | Medium | Predictable usage |
| Spot | Very Low | Low | Interruptible workloads |
| Dedicated Host | Very High | Medium | Compliance and licensing |

---

# Additional EC2 Costs

Launching an EC2 instance may involve charges beyond compute.

### Amazon EBS

- Root volume
- Additional volumes
- Snapshots

### Data Transfer

- Internet egress
- Cross-region transfers

### Elastic IP

Free while properly attached to a running instance in many common scenarios, but charges can apply for unused or additional public IPv4 addresses.

### CloudWatch

- Basic Monitoring
- Detailed Monitoring
- Logs
- Custom Metrics

---

# Cost Optimization Tips

### Right-Size Instances

Choose the smallest instance that meets workload requirements.

Example

❌ m6i.4xlarge for a small website

✅ t3.micro or t3.small

---

### Stop Unused Instances

Development servers should be stopped when not in use.

Remember:

Stopping an instance stops compute charges, but EBS storage charges continue.

---

### Use Auto Scaling

Automatically launch and terminate instances based on demand.

Benefits

- Reduced cost
- Better resource utilization

---

### Delete Unused Resources

Regularly remove:

- Unused EBS volumes
- Old snapshots
- Unused Elastic IPs
- Unused AMIs

---

### Monitor with AWS Cost Explorer

Use Cost Explorer to:

- Track spending
- Analyze trends
- Forecast future costs
- Identify optimization opportunities

---

# Real Production Example

A company hosts an e-commerce application.

```
Production Servers

↓

Reserved Instances / Savings Plans

↓

Development

↓

On-Demand

↓

Nightly Batch Jobs

↓

Spot Instances

↓

Compliance Server

↓

Dedicated Host
```

Each workload uses the most cost-effective pricing model.

---

# Best Practices

✔ Use On-Demand for testing and learning.

✔ Use Savings Plans or Reserved Instances for predictable production workloads.

✔ Use Spot Instances for fault-tolerant jobs.

✔ Monitor spending regularly.

✔ Right-size instances based on CloudWatch metrics.

✔ Delete unused resources.

---

# Common Mistakes

❌ Leaving development instances running overnight.

❌ Using On-Demand for always-on production workloads.

❌ Forgetting to delete unattached EBS volumes.

❌ Ignoring data transfer costs.

❌ Choosing oversized instance types.

---

# Interview Questions

## Beginner

1. What is On-Demand pricing?
2. What are Spot Instances?
3. What is a Reserved Instance?
4. What is a Savings Plan?
5. Which pricing option is cheapest?

---

## Intermediate

1. Compare On-Demand and Reserved Instances.
2. When should you use Spot Instances?
3. What costs are associated with EC2 besides compute?
4. How can you reduce EC2 costs?
5. Explain EC2 pricing models with examples.

---

# Quick Revision

```
On-Demand

↓

Reserved Instances

↓

Savings Plans

↓

Spot Instances

↓

Dedicated Hosts
```

Remember:

- On-Demand → Flexible
- Reserved → Long-term commitment
- Savings Plans → Flexible long-term savings
- Spot → Cheapest, interruptible
- Dedicated Hosts → Compliance

---

# Summary

Amazon EC2 provides multiple pricing options to balance flexibility and cost. On-Demand is ideal for short-term or unpredictable workloads, Reserved Instances and Savings Plans reduce costs for long-running workloads, Spot Instances provide significant savings for interruptible tasks, and Dedicated Hosts support compliance and licensing requirements. Choosing the right pricing model is a key part of cost optimization in AWS.

---

# Related Topics

- EC2 Instance Types
- Auto Scaling
- Amazon EBS
- AWS Cost Explorer
- AWS Budgets

---

# References

- AWS EC2 Pricing: https://aws.amazon.com/ec2/pricing/
- AWS Savings Plans: https://aws.amazon.com/savingsplans/
- AWS Cost Explorer: https://aws.amazon.com/aws-cost-management/cost-explorer/