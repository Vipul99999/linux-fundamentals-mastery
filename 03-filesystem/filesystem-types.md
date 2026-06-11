# Linux Filesystem Types

> Linux supports many filesystem types, each designed for different use cases.
>
> Some filesystems are optimized for:
>
> * General-purpose computing
> * Large storage systems
> * Enterprise workloads
> * Containers
> * Embedded systems
> * Memory-based storage
> * Network storage
>
> Understanding filesystem types helps administrators choose the right filesystem for performance, reliability, scalability, and data integrity.

---

# Table of Contents

1. What is a Filesystem?
2. Why Multiple Filesystems Exist
3. Filesystem Categories
4. Linux Filesystem Family Tree
5. Local Filesystems
6. Network Filesystems
7. Virtual Filesystems
8. Memory-Based Filesystems
9. Cluster Filesystems
10. Filesystem Comparison
11. Choosing a Filesystem
12. Real World Usage
13. Interview Questions

---

# What is a Filesystem?

A filesystem is a method used by the operating system to:

```text
Store Data
Organize Data
Retrieve Data
Manage Metadata
```

---

# Visual

```text
Application
      │
      ▼
Filesystem
      │
      ▼
Storage Device
```

---

# Why Multiple Filesystems Exist

Different workloads have different needs.

Example:

```text
Desktop User
      │
      ▼
Simple + Reliable
```

---

```text
Database Server
      │
      ▼
High Throughput
```

---

```text
Backup Server
      │
      ▼
Snapshots + Compression
```

---

Therefore Linux supports multiple filesystem types.

---

# Filesystem Categories

```text
Linux Filesystems
│
├── Local Filesystems
├── Network Filesystems
├── Virtual Filesystems
├── Memory Filesystems
└── Cluster Filesystems
```

---

# Linux Filesystem Family Tree

```text
Linux Filesystems
│
├── ext2
├── ext3
├── ext4
├── XFS
├── Btrfs
├── ZFS
│
├── NFS
├── SMB
│
├── procfs
├── sysfs
├── devtmpfs
│
├── tmpfs
│
└── CephFS
```

---

# Local Filesystems

Stored on local disks.

---

## ext2

Second Extended Filesystem.

Features:

```text
No Journaling
Simple
Lightweight
```

---

Visual

```text
ext2
 │
 ▼
Basic Filesystem
```

---

### Advantages

✅ Simple

✅ Low Overhead

---

### Disadvantages

❌ No Journaling

❌ Slow Recovery

---

# ext3

Added journaling.

---

Features:

```text
Journaling
Backward Compatible
```

---

Visual

```text
ext2
 │
 ▼
Journaling
 │
 ▼
ext3
```

---

# ext4

Most widely used Linux filesystem.

---

Features:

```text
Journaling
Extents
Large Volumes
High Performance
```

---

Visual

```text
ext4
│
├── Journaling
├── Extents
├── HTree
└── Delayed Allocation
```

---

Use Cases:

```text
Desktop
Laptop
General Servers
```

---

# XFS

Enterprise filesystem.

---

Features:

```text
Allocation Groups
Extents
Journaling
Scalability
```

---

Visual

```text
XFS
│
├── Allocation Groups
├── Journaling
└── Extents
```

---

Use Cases:

```text
Large Files
Databases
Enterprise Storage
```

---

# Btrfs

Modern next-generation filesystem.

---

Features:

```text
Snapshots
Checksums
Compression
RAID
Subvolumes
```

---

Visual

```text
Btrfs
│
├── Snapshots
├── Checksums
├── CoW
├── RAID
└── Compression
```

---

Use Cases:

```text
Backups
Snapshots
Containers
```

---

# ZFS

Advanced storage filesystem.

---

Features:

```text
Snapshots
Checksums
Compression
Storage Pools
Self-Healing
```

---

Visual

```text
ZFS
│
├── Pools
├── Snapshots
├── Compression
└── Checksums
```

---

Use Cases:

```text
Enterprise Storage
NAS
Large Data Systems
```

---

# FAT32

Cross-platform filesystem.

---

Features:

```text
Simple
Universal Support
```

---

Limitations:

```text
4GB File Limit
```

---

Used for:

```text
USB Drives
Memory Cards
```

---

# exFAT

Modern FAT replacement.

---

Features:

```text
Large File Support
Cross Platform
```

---

Used for:

```text
USB
External Drives
SD Cards
```

---

# NTFS

Windows filesystem.

---

Linux can:

```text
Read
Write
Mount
```

NTFS volumes.

---

Used for:

```text
Windows Partitions
External Drives
```

---

# Network Filesystems

Access files across network.

---

Visual

```text
Client
  │
  ▼
Network
  │
  ▼
Remote Storage
```

---

# NFS

Network File System.

---

Visual

```text
Linux Client
      │
      ▼
NFS Server
```

---

Use Cases:

```text
Shared Storage
Linux Clusters
```

---

# SMB/CIFS

Used by Windows.

---

Visual

```text
Windows
Linux
Mac
 │
 ▼
SMB Server
```

---

Use Cases:

```text
File Sharing
Mixed Environments
```

---

# SSHFS

Filesystem over SSH.

---

Visual

```text
SSH
 │
 ▼
Remote Filesystem
```

---

Benefits:

```text
Secure
Easy Setup
```

---

# Virtual Filesystems

Generated dynamically by kernel.

---

# procfs

Mount Point:

```text
/proc
```

Contains:

```text
Process Information
Kernel Information
```

---

# sysfs

Mount Point:

```text
/ sys
```

Contains:

```text
Devices
Drivers
Hardware
```

---

# devtmpfs

Mount Point:

```text
/dev
```

Contains:

```text
Device Files
```

---

Visual

```text
Kernel
 │
 ▼
Virtual Filesystem
 │
 ▼
Files
```

---

# Memory-Based Filesystems

Stored in RAM.

---

# tmpfs

Mount Point:

```text
/tmp
/run
/dev/shm
```

---

Visual

```text
RAM
 │
 ▼
tmpfs
```

---

Benefits:

```text
Very Fast
Temporary
```

---

# OverlayFS

Container filesystem.

---

Visual

```text
Upper Layer
      │
      ▼
Overlay
      │
      ▼
Lower Layer
```

---

Used by:

```text
Docker
Podman
```

---

# Cluster Filesystems

Shared by multiple servers.

---

# CephFS

Distributed filesystem.

---

Visual

```text
Server A
Server B
Server C
     │
     ▼
Ceph Cluster
```

---

# GlusterFS

Distributed storage.

---

Visual

```text
Multiple Nodes
      │
      ▼
Single Filesystem
```

---

# Filesystem Comparison

| Filesystem | Journaling | Snapshots | Checksums | Compression |
| ---------- | ---------- | --------- | --------- | ----------- |
| ext2       | No         | No        | No        | No          |
| ext3       | Yes        | No        | No        | No          |
| ext4       | Yes        | No        | No        | No          |
| XFS        | Yes        | No        | No        | No          |
| Btrfs      | Yes        | Yes       | Yes       | Yes         |
| ZFS        | Yes        | Yes       | Yes       | Yes         |

---

# Choosing a Filesystem

## Desktop Linux

Recommended:

```text
ext4
```

---

## Enterprise Servers

Recommended:

```text
XFS
```

---

## Snapshots

Recommended:

```text
Btrfs
ZFS
```

---

## USB Drives

Recommended:

```text
exFAT
```

---

## Shared Storage

Recommended:

```text
NFS
SMB
```

---

## Containers

Recommended:

```text
OverlayFS
Btrfs
```

---

# Real World Usage

```text
Ubuntu
   │
   ▼
 ext4
```

---

```text
RHEL
  │
  ▼
 XFS
```

---

```text
Fedora
   │
   ▼
 Btrfs
```

---

```text
Docker
   │
   ▼
 OverlayFS
```

---

```text
Kubernetes
     │
     ▼
 OverlayFS + Network Storage
```

---

# Complete Filesystem Map

```text
Linux Filesystems
│
├── Local
│   ├── ext2
│   ├── ext3
│   ├── ext4
│   ├── XFS
│   ├── Btrfs
│   └── ZFS
│
├── Network
│   ├── NFS
│   ├── SMB
│   └── SSHFS
│
├── Virtual
│   ├── procfs
│   ├── sysfs
│   └── devtmpfs
│
├── Memory
│   ├── tmpfs
│   └── OverlayFS
│
└── Cluster
    ├── CephFS
    └── GlusterFS
```

---

# Interview Questions

## Beginner

1. What is a filesystem?
2. Why do multiple filesystem types exist?
3. What is ext4?
4. What is XFS?
5. What is Btrfs?

---

## Intermediate

6. Compare ext4 and XFS.
7. Compare ext4 and Btrfs.
8. Explain journaling.
9. Explain snapshots.
10. Explain checksums.

---

## Advanced

11. Explain Copy-on-Write.
12. Explain distributed filesystems.
13. Explain OverlayFS.
14. Explain filesystem scalability.
15. Explain VFS interactions.

---

# Summary

```text
Filesystem Types
│
├── ext4
├── XFS
├── Btrfs
├── ZFS
├── NFS
├── SMB
├── procfs
├── sysfs
├── tmpfs
└── OverlayFS
```

Key Idea:

```text
No Filesystem Is Best For Everything
```

Choose based on:

```text
Performance
Reliability
Scalability
Snapshots
Storage Requirements
Workload Type
```

Understanding filesystem types is the foundation for learning:

* Storage Management
* Linux Administration
* Containers
* Cloud Computing
* Kubernetes
* Distributed Storage
* Linux Kernel Internals
* Enterprise Infrastructure

```
```
