# Amazon EC2 Interview Questions

> **Document Version:** 1.0  
> **Category:** AWS Compute  
> **Service:** Amazon EC2  
> **Difficulty:** Beginner to Advanced  
> **Estimated Reading Time:** 60 Minutes

---

# Objective

This document contains commonly asked Amazon EC2 interview questions ranging from beginner to advanced level. It covers theoretical concepts, practical scenarios, troubleshooting, AWS CLI, architecture, and production best practices.

---

# Beginner Level Questions

## EC2 Basics

1. What is Amazon EC2?
2. What does EC2 stand for?
3. What are the advantages of EC2?
4. Why do companies use EC2?
5. What is an EC2 instance?
6. What is virtualization?
7. What is an Amazon Machine Image (AMI)?
8. What information does an AMI contain?
9. What is an Instance Type?
10. What is a Key Pair?

---

## EC2 Components

11. What is an Elastic IP?
12. What is a Public IP?
13. What is a Private IP?
14. What is an Availability Zone?
15. What is a Region?
16. What is a VPC?
17. What is a Subnet?
18. What is a Security Group?
19. What is a Network ACL?
20. What is an IAM Role?

---

## EC2 Lifecycle

21. Explain the EC2 lifecycle.
22. Difference between Stop and Terminate?
23. What happens when an EC2 instance is rebooted?
24. Can you recover a terminated instance?
25. What happens to the Public IP after stopping an instance?
26. What happens to the Private IP after stopping an instance?
27. Which storage survives instance stop?
28. What happens during instance launch?
29. What is the boot process of EC2?
30. What is User Data?

---

# Intermediate Level Questions

## Instance Types

31. Explain EC2 instance families.
32. Difference between T, M, C, R, and I families?
33. When should you choose Compute Optimized instances?
34. Which instance family is suitable for databases?
35. Which instance family is suitable for machine learning?
36. What are Graviton instances?
37. Difference between ARM and x86 instances?
38. How do you resize an EC2 instance?
39. What factors influence instance selection?
40. How do you monitor instance performance?

---

## Storage

41. What is Amazon EBS?
42. Difference between EBS and Instance Store?
43. Difference between gp2 and gp3?
44. What is an EBS Snapshot?
45. Can you increase EBS volume size?
46. What happens if the root volume is deleted?
47. Can multiple EC2 instances attach to one EBS volume?
48. What is Multi-Attach?
49. What is EBS encryption?
50. Why are snapshots incremental?

---

## Networking

51. How does an EC2 instance connect to the internet?
52. What is an Internet Gateway?
53. What is a Route Table?
54. Explain Security Group vs NACL.
55. What ports are commonly opened on EC2?
56. How do you make an EC2 instance private?
57. Why use a Bastion Host?
58. What is an Elastic Network Interface (ENI)?
59. What is Source/Destination Check?
60. What is VPC Flow Log?

---

# Advanced Level Questions

## High Availability

61. How do you make EC2 highly available?
62. Why use multiple Availability Zones?
63. What is Auto Scaling?
64. How does an Auto Scaling Group work?
65. What is a Launch Template?
66. Difference between Launch Configuration and Launch Template?
67. What is Elastic Load Balancing?
68. Explain an EC2 production architecture.
69. What is health checking?
70. What happens when an instance becomes unhealthy?

---

## Security

71. Explain the Shared Responsibility Model.
72. Why should IAM Roles be used?
73. Why should Access Keys not be stored on EC2?
74. What is IMDSv2?
75. What is Session Manager?
76. How do you secure SSH?
77. How do you encrypt EBS volumes?
78. What is AWS KMS?
79. What is AWS GuardDuty?
80. What is AWS Inspector?

---

## Monitoring

81. What metrics does CloudWatch provide?
82. How do you monitor memory usage?
83. How do you monitor disk usage?
84. What is CloudTrail?
85. Difference between CloudWatch and CloudTrail?
86. What alarms would you create for production?
87. How do you investigate high CPU?
88. How do you troubleshoot disk full?
89. Which logs should be collected?
90. What is the AWS Health Dashboard?

---

# Troubleshooting Questions

91. You cannot SSH into EC2. What will you check?
92. Website hosted on EC2 is not opening. How do you troubleshoot?
93. CPU utilization is continuously above 95%. What steps will you take?
94. An EBS volume is not mounting. What will you verify?
95. The instance has become unreachable. How do you recover it?
96. The application is slow only during peak hours. What could be the reason?
97. Security Group is correct, but traffic is still blocked. What else would you check?
98. Auto Scaling is not launching new instances. How would you investigate?
99. Load Balancer health checks are failing. What could be the causes?
100. A production EC2 instance accidentally lost its key pair. How can access be restored?

---

# AWS CLI Questions

101. How do you list all EC2 instances using AWS CLI?
102. Which command launches an EC2 instance?
103. How do you stop an EC2 instance using CLI?
104. How do you terminate an EC2 instance?
105. How do you describe instance status?
106. How do you create an AMI?
107. How do you create an EBS snapshot?
108. How do you allocate an Elastic IP?
109. How do you tag an EC2 instance?
110. How do you filter EC2 instances by tag?

---

# Scenario-Based Questions

111. Design a highly available web application using EC2.
112. How would you migrate a web application from on-premises to EC2?
113. Your company expects traffic to increase 10× during a sale. How would you prepare?
114. How would you reduce EC2 costs without affecting performance?
115. How would you secure production EC2 instances?
116. How would you design a disaster recovery strategy?
117. How would you deploy an application with zero downtime?
118. How would you recover from an Availability Zone failure?
119. How would you investigate unexpected AWS billing related to EC2?
120. Explain an architecture you have worked on using EC2.

---

# HR + Technical Discussion

121. Describe your experience with Amazon EC2.
122. Have you launched an EC2 instance? Explain the steps.
123. What production issues have you handled on EC2?
124. Which monitoring tools have you used?
125. How do you handle critical production incidents?
126. Describe a challenging EC2 issue you resolved.
127. How do you prioritize incidents?
128. Have you used Auto Scaling in production?
129. Which AWS services have you integrated with EC2?
130. Why should we hire you for an AWS/DevOps role?

---

# Interview Tips

- Understand the complete EC2 launch process.
- Practice creating EC2 instances using both Console and AWS CLI.
- Learn common Linux troubleshooting commands.
- Be able to explain networking concepts (VPC, Subnets, Security Groups, NACLs).
- Prepare real production examples and incident stories.
- Revise EC2 pricing models and instance families.
- Be comfortable drawing simple AWS architectures on a whiteboard.

---

# Quick Revision

```
EC2 Basics
↓

Instance Types
↓

AMI

↓

Networking

↓

Storage

↓

Security

↓

Monitoring

↓

Auto Scaling

↓

Load Balancer

↓

Troubleshooting

↓

AWS CLI

↓

Production Scenarios
```

---

# Summary

Strong EC2 interview preparation requires understanding not only theoretical concepts but also practical implementation, troubleshooting, security, monitoring, and production architecture. Regular hands-on practice in the AWS Console and AWS CLI, combined with knowledge of real-world scenarios, will help you confidently answer technical interview questions.

---

# Related Topics

- Amazon EBS
- VPC
- IAM
- Auto Scaling
- Elastic Load Balancer
- CloudWatch
- CloudTrail
- AWS Systems Manager
- AWS CLI

---

# References

- AWS EC2 User Guide
- AWS Well-Architected Framework
- AWS Skill Builder
- AWS Solutions Architect Associate (SAA-C03) Exam Guide