# Amazon EBS Security

> **Document Version:** 1.0
> **Category:** AWS Storage
> **Service:** Amazon Elastic Block Store (EBS)
> **Difficulty:** Intermediate to Advanced
> **Estimated Reading Time:** 40 Minutes

---

# Objective

After reading this document, you will understand:

- Amazon EBS security fundamentals
- Encryption at Rest
- Encryption in Transit
- AWS KMS integration
- IAM access control
- Snapshot security
- Cross-account snapshot sharing
- Monitoring and auditing
- Compliance
- Production security best practices

---

# Introduction

Amazon EBS stores critical application and business data.

Because EBS often contains operating systems, databases, application files, and customer information, securing EBS is an essential part of any AWS environment.

Security should cover:

- Access Control
- Encryption
- Monitoring
- Backup Protection
- Compliance
- Auditing

---

# EBS Security Architecture

```
User

↓

IAM Authentication

↓

AWS Account

↓

EC2 Instance

↓

Encrypted Amazon EBS Volume

↓

AWS KMS

↓

CloudTrail

↓

CloudWatch
```

---

# Shared Responsibility Model

AWS secures:

- Physical Data Centers
- Storage Hardware
- Networking Infrastructure
- Hypervisor

Customer secures:

- IAM Policies
- Encryption Configuration
- Snapshots
- Data
- Operating System
- Applications

---

# Encryption at Rest

Amazon EBS supports encryption using AWS Key Management Service (KMS).

Encrypted resources include:

- Root Volumes
- Data Volumes
- Snapshots
- Volumes created from encrypted snapshots

Benefits

- Protects stored data
- Meets compliance requirements
- Transparent to applications

---

# AWS KMS Integration

Encryption keys are managed by AWS KMS.

Options

- AWS Managed Keys
- Customer Managed Keys (CMKs)

Customer-managed keys provide:

- Key rotation
- Fine-grained permissions
- Audit logs
- Key lifecycle management

---

# Encryption Workflow

```
Application

↓

EC2 Instance

↓

Encrypted EBS Volume

↓

AWS KMS Key

↓

Encrypted Data
```

Applications do not need to perform encryption manually.

---

# Encryption in Transit

When data moves between:

- EC2
- EBS

AWS encrypts communication on supported instance families.

Benefits

- Protects against interception
- Improves data confidentiality

---

# IAM Access Control

Control EBS access using IAM.

Examples

Allow

- Create Volume
- Attach Volume
- Create Snapshot

Deny

- Delete Volume
- Delete Snapshot

Always follow the Principle of Least Privilege.

---

# Snapshot Security

Snapshots contain complete copies of your data over time.

Protect them by:

- Encrypting snapshots
- Restricting sharing
- Applying IAM policies
- Tagging sensitive backups

---

# Snapshot Sharing

Snapshots may be shared with another AWS account.

Never share snapshots containing:

- Customer information
- Passwords
- Private keys
- Financial records

If sharing encrypted snapshots:

- The target account must have access to the KMS key.

---

# Public Snapshots

Never make production snapshots public.

Risk

```
Sensitive Data

↓

Public Snapshot

↓

Unauthorized Access
```

Always verify snapshot permissions.

---

# Secure Volume Attachment

Attach EBS volumes only to authorized EC2 instances.

Verify

- Correct Instance
- Correct Availability Zone
- Correct IAM permissions

---

# Multi-Attach Security

Multi-Attach is supported only for compatible io1/io2 volumes.

Use only when required.

Applications must support concurrent access.

Improper configuration may result in data corruption.

---

# Tagging for Security

Recommended Tags

| Key | Value |
|------|-------|
| Name | DatabaseVolume |
| Environment | Production |
| Classification | Confidential |
| Backup | Daily |
| Owner | DevOps |

Benefits

- Resource identification
- Compliance
- Automation
- Cost allocation

---

# Monitoring

Monitor EBS using Amazon CloudWatch.

Key Metrics

- Read Ops
- Write Ops
- Read Latency
- Write Latency
- Queue Length

Create alarms for:

- High latency
- Abnormal activity
- Performance degradation

---

# Auditing

Use AWS CloudTrail.

CloudTrail records:

- CreateVolume
- DeleteVolume
- AttachVolume
- DetachVolume
- CreateSnapshot
- DeleteSnapshot
- ModifyVolume

Useful for

- Compliance
- Incident investigation
- Change tracking

---

# Compliance

EBS encryption helps organizations comply with:

- PCI DSS
- HIPAA
- SOC 2
- ISO 27001
- GDPR

Always verify compliance requirements before deployment.

---

# Backup Security

Protect backups by:

- Encrypting snapshots
- Limiting IAM access
- Using AWS Backup
- Copying snapshots to another Region
- Testing restores regularly

---

# Production Security Architecture

```
IAM

↓

EC2

↓

Encrypted gp3/io2 Volume

↓

AWS KMS

↓

Daily Snapshot

↓

Encrypted Snapshot

↓

AWS Backup

↓

Cross-Region Copy

↓

Disaster Recovery
```

---

# Security Best Practices

- Encrypt all EBS volumes.
- Use Customer Managed KMS Keys for sensitive workloads.
- Apply Least Privilege IAM policies.
- Enable CloudTrail.
- Enable CloudWatch alarms.
- Tag all volumes.
- Encrypt snapshots.
- Test backup restoration.
- Rotate KMS keys according to policy.
- Review IAM permissions regularly.

---

# Production Security Checklist

Before deployment:

- Encryption enabled
- KMS key selected
- IAM policy reviewed
- Snapshot policy configured
- CloudTrail enabled
- CloudWatch monitoring enabled
- Backup encryption verified
- Tags applied
- Compliance requirements checked

---

# Common Security Mistakes

❌ Leaving production volumes unencrypted

❌ Using AdministratorAccess for all users

❌ Sharing snapshots publicly

❌ Ignoring CloudTrail logs

❌ Not rotating KMS keys

❌ Forgetting to encrypt restored volumes

❌ Not testing backup recovery

---

# Interview Questions

## Beginner

1. Why should Amazon EBS volumes be encrypted?
2. What service provides EBS encryption keys?
3. Can encrypted snapshots be shared?
4. What is AWS KMS?
5. Why is CloudTrail important?

---

## Intermediate

1. Explain encryption at rest vs encryption in transit.
2. How do IAM policies protect EBS?
3. How would you secure production snapshots?
4. What are Customer Managed KMS Keys?
5. How would you audit EBS activity?

---

## Advanced

1. Design a secure EBS architecture for a banking application.
2. Explain a disaster recovery strategy for encrypted EBS volumes.
3. How would you securely share encrypted snapshots across AWS accounts?
4. What security controls would you implement for compliance?
5. How would you investigate unauthorized EBS changes?

---

# Quick Revision

```
IAM

↓

KMS

↓

Encryption

↓

Snapshots

↓

CloudTrail

↓

CloudWatch

↓

AWS Backup

↓

Compliance

↓

Disaster Recovery
```

---

# Summary

Amazon EBS security is built on multiple layers, including IAM access control, AWS KMS encryption, CloudTrail auditing, CloudWatch monitoring, and secure snapshot management. By encrypting all volumes, protecting backups, applying least-privilege permissions, and monitoring changes, organizations can build secure and compliant storage solutions for production workloads.

---

# Related Topics

- Amazon EC2
- AWS KMS
- AWS Backup
- CloudWatch
- CloudTrail
- IAM
- Amazon EBS Snapshots

---

# References

- Amazon EBS User Guide
- AWS KMS Developer Guide
- AWS Security Best Practices
- AWS Well-Architected Framework – Security Pillar