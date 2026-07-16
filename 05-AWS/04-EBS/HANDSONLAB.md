# Amazon EFS (Elastic File System) Hands-On Lab

## What is EBS and EFS?

### EBS (Elastic Block Store)
- **Full Form:** Elastic Block Store
- Stores data as **Blocks**
- Supports **Linux & Windows**
- **Availability Zone (AZ) Specific**
- Can be used as a **Boot Volume**
- **No port required**
- Main Purpose:
  - Increase EC2 storage
  - Store Operating System
  - High-performance block storage

---

### EFS (Elastic File System)
- **Full Form:** Elastic File System
- Stores data as **Files**
- Supports **Linux only**
- **Region Specific**
- Cannot be used as a **Boot Volume**
- Uses **NFS (Network File System)**
- **Port:** **2049**
- Main Purpose:
  - Share files between multiple Linux EC2 instances
  - Keep directories synchronized
  - Centralized shared storage

---

# EBS vs EFS

| Feature | EBS | EFS |
|----------|-----|-----|
| Full Form | Elastic Block Store | Elastic File System |
| Storage Type | Block Storage | File Storage |
| OS Support | Linux & Windows | Linux Only |
| Scope | Availability Zone | Region |
| Bootable | Yes | No |
| Port | No Port | 2049 (NFS) |
| Share with Multiple EC2 | No | Yes |
| Main Use | Extra Disk | Shared File Storage |

---

# Practical: Create Amazon EFS

## Step 1: Create Security Group

Create a Security Group with the following inbound rules.

| Port | Protocol | Purpose |
|------|----------|----------|
| 22 | SSH | Connect to EC2 |
| 2049 | NFS | Mount EFS |

---

## Step 2: Launch EC2 Instances

Create **2 Linux EC2 instances**.

Example:

- EC2-1
- EC2-2

Both instances should:

- Be in the **same VPC**
- Use the **same Security Group**
- Be in any Availability Zone of the same Region

---

## Step 3: Connect to Both EC2 Instances

SSH into both instances.

Example:

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

---

## Step 4: Create EFS File System

Go to:

```
AWS Console
    ↓
Elastic File System (EFS)
    ↓
Create File System
```

Choose:

- Name
- VPC
- Keep default settings (or customize if required)

Click:

```
Create
```

---

## Step 5: Create Mount Targets

Ensure Mount Targets are created for the required Availability Zones.

Security Group should allow:

- Port **2049**

---

## Step 6: Copy the Mount Command

Open your EFS.

Click:

```
Attach
```

Choose:

```
Linux
```

AWS provides the mount command.

Example:

```bash
sudo mount -t efs fs-xxxxxxxx:/ /mnt/efs
```

---

## Step 7: Install Amazon EFS Utilities (if required)

### Amazon Linux

```bash
sudo dnf install amazon-efs-utils -y
```

### Ubuntu

```bash
sudo apt update
sudo apt install amazon-efs-utils -y
```

---

## Step 8: Create Mount Directory

```bash
sudo mkdir /mnt/efs
```

---

## Step 9: Mount EFS

Example:

```bash
sudo mount -t efs fs-xxxxxxxx:/ /mnt/efs
```

Verify:

```bash
df -h
```

---

## Step 10: Test from EC2-1

Create a file.

```bash
cd /mnt/efs

echo "Hello from EC2-1" > test.txt
```

Check:

```bash
ls

cat test.txt
```

---

## Step 11: Mount EFS on EC2-2

Repeat the same mount steps.

```bash
sudo mkdir /mnt/efs

sudo mount -t efs fs-xxxxxxxx:/ /mnt/efs
```

---

## Step 12: Verify Synchronization

On EC2-2

```bash
cd /mnt/efs

ls

cat test.txt
```

Output:

```
Hello from EC2-1
```

This proves that both EC2 instances are using the same shared file system.

---

# Verify Mounted File System

```bash
df -h
```

or

```bash
mount | grep efs
```

---

# Unmount EFS

```bash
sudo umount /mnt/efs
```

---

# Mount Automatically After Reboot

Open:

```bash
sudo vi /etc/fstab
```

Add:

```text
fs-xxxxxxxx:/ /mnt/efs efs defaults,_netdev 0 0
```

Save and verify:

```bash
sudo mount -a
```

---

# Architecture

```
              AWS Region
                   │
        ┌─────────────────────┐
        │     Amazon EFS      │
        └─────────┬───────────┘
                  │
       ┌──────────┴──────────┐
       │                     │
   Port 2049             Port 2049
       │                     │
 ┌────────────┐        ┌────────────┐
 │   EC2-1    │        │   EC2-2    │
 │ Linux      │        │ Linux      │
 │ /mnt/efs   │        │ /mnt/efs   │
 └────────────┘        └────────────┘
```

---

# Important Interview Points

- EFS is a managed NFS service.
- Supports only Linux instances.
- Region-wide service.
- Can be mounted on multiple EC2 instances simultaneously.
- Uses port **2049**.
- Data is automatically synchronized across all mounted instances.
- Highly available and scalable.
- Storage grows and shrinks automatically.
- No need to provision storage capacity in advance.

---

# Summary

- **EBS** = Block Storage (Single EC2, Boot Volume, AZ-specific)
- **EFS** = File Storage (Multiple Linux EC2s, Shared Storage, Region-specific)
- **EFS Port:** 2049 (NFS)
- **Main Use Case:** Share files and directories across multiple Linux EC2 instances.