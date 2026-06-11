# XFS Filesystem in Linux

> XFS is a high-performance 64-bit journaling filesystem originally developed by Silicon Graphics (SGI).
>
> It is designed for:
>
> * Large files
> * Large filesystems
> * High-throughput workloads
> * Parallel I/O
> * Enterprise storage systems
>
> XFS is the default filesystem in many enterprise Linux distributions such as RHEL, Rocky Linux, AlmaLinux, and Oracle Linux.

---

# Table of Contents

1. What is XFS?
2. Why XFS Exists
3. XFS Architecture
4. Allocation Groups
5. Inodes
6. Extents
7. Journaling
8. Delayed Allocation
9. Scalability
10. Performance Characteristics
11. XFS Features
12. Common Commands
13. Real World Usage
14. XFS vs ext4
15. XFS vs Btrfs
16. Troubleshooting
17. Interview Questions

---

# What is XFS?

XFS is a:

```text
64-bit
Journaling
High-Performance
Filesystem
```

optimized for enterprise workloads.

---

# Why XFS Exists

Traditional filesystems struggled with:

```text
Huge Storage Arrays
Large Files
Parallel Workloads
High Throughput
```

XFS was designed to solve these issues.

---

# XFS Architecture

```text
Filesystem
│
├── Superblocks
├── Allocation Groups
├── Inodes
├── Extents
└── Journal
```

---

# Allocation Groups (AGs)

The most important XFS concept.

Instead of treating the filesystem as one giant structure:

```text
Filesystem
│
├── AG0
├── AG1
├── AG2
└── AG3
```

Each AG can operate independently.

---

# Benefit

Multiple CPUs can allocate space simultaneously.

```text
CPU 1 → AG0
CPU 2 → AG1
CPU 3 → AG2
CPU 4 → AG3
```

This dramatically improves scalability.

---

# Inodes

Each file gets an inode.

Stores:

```text
Permissions
Owner
Timestamps
Extent Information
```

---

# Extents

XFS uses extents.

Instead of:

```text
Block 1
Block 2
Block 3
Block 4
```

XFS stores:

```text
Start Block = 1000
Length = 500
```

Benefits:

```text
Less Metadata
Less Fragmentation
Faster Access
```

---

# Journaling

XFS journals metadata.

```text
Metadata Change
      │
      ▼
Journal
      │
      ▼
Disk
```

Benefits:

```text
Crash Recovery
Consistency
```

---

# Delayed Allocation

XFS delays physical block allocation.

Instead of:

```text
Write
Allocate
Write
Allocate
```

it performs:

```text
Write
Write
Write
Allocate Later
```

Benefits:

```text
Better Layout
Less Fragmentation
Higher Throughput
```

---

# Scalability

XFS was designed for:

```text
Multi-Core Systems
Large RAID Arrays
Massive Storage Pools
```

Supports:

```text
Very Large Files
Very Large Filesystems
```

---

# Performance Characteristics

## Strengths

✅ Excellent sequential I/O

✅ Excellent large file performance

✅ Excellent parallelism

✅ Enterprise-grade stability

✅ Fast metadata operations

---

## Weaknesses

❌ No native snapshots

❌ No built-in checksums for user data

❌ No built-in compression

❌ No built-in RAID management

---

# XFS Features

## Journaling

```text
Crash Recovery
```

---

## Extents

```text
Reduced Fragmentation
```

---

## Allocation Groups

```text
Parallel Scalability
```

---

## Delayed Allocation

```text
Performance Optimization
```

---

## Online Expansion

Filesystem can grow while mounted.

```bash
xfs_growfs
```

---

# Creating XFS

```bash
mkfs.xfs /dev/sdb1
```

---

# Mounting XFS

```bash
mount -t xfs /dev/sdb1 /mnt
```

---

# View XFS Information

```bash
xfs_info /mnt
```

---

# Repair XFS

```bash
xfs_repair /dev/sdb1
```

---

# Grow Filesystem

```bash
xfs_growfs /mnt
```

---

# Real World Usage

## Enterprise Servers

```text
RHEL
Rocky Linux
AlmaLinux
Oracle Linux
```

---

## Databases

```text
PostgreSQL
MySQL
Oracle
```

---

## Media Storage

```text
Video Files
Backup Archives
Large Datasets
```

---

## Virtualization

```text
VMware
KVM
OpenStack
```

---

# XFS vs ext4

| Feature              | XFS       | ext4      |
| -------------------- | --------- | --------- |
| Journaling           | Yes       | Yes       |
| Snapshots            | No        | No        |
| Large Files          | Excellent | Good      |
| Scalability          | Excellent | Good      |
| Enterprise Workloads | Excellent | Good      |
| Small Systems        | Good      | Excellent |

---

# XFS vs Btrfs

| Feature     | XFS       | Btrfs    |
| ----------- | --------- | -------- |
| Snapshots   | No        | Yes      |
| Checksums   | Limited   | Yes      |
| Compression | No        | Yes      |
| RAID        | External  | Built-in |
| Complexity  | Medium    | High     |
| Throughput  | Excellent | Good     |

---

# Troubleshooting

## View Filesystem

```bash
xfs_info /mnt
```

---

## Check Usage

```bash
df -h
```

---

## Repair Filesystem

```bash
xfs_repair /dev/sdb1
```

---

## Show Mounts

```bash
mount | grep xfs
```

---

# Interview Questions

## Beginner

1. What is XFS?
2. Who developed XFS?
3. What is journaling?
4. What are extents?
5. What is an allocation group?

---

## Intermediate

6. Explain XFS architecture.
7. Explain delayed allocation.
8. Explain allocation groups.
9. Explain XFS scalability.
10. Explain XFS journaling.

---

## Advanced

11. Explain extent allocation.
12. Explain AG locking.
13. Explain XFS metadata management.
14. Explain online expansion.
15. Explain XFS performance design.

---

# Summary

```text
XFS
│
├── Allocation Groups
├── Extents
├── Inodes
├── Journaling
└── Delayed Allocation
```

Core Idea:

```text
Application
      │
      ▼
VFS
      │
      ▼
XFS
      │
      ▼
Allocation Groups
      │
      ▼
Storage
```

XFS is optimized for:

```text
Large Files
Large Filesystems
Parallel Workloads
Enterprise Storage
High Throughput
```
