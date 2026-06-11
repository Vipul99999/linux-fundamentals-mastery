# ext4 Filesystem in Linux

> ext4 (Fourth Extended Filesystem) is the most widely used Linux filesystem.
>
> It is the default filesystem on many Linux distributions including:
>
> - Ubuntu
> - Debian
> - Linux Mint
> - CentOS (older)
> - RHEL (older)
>
> ext4 is known for its:
>
> ✅ Stability  
> ✅ Performance  
> ✅ Reliability  
> ✅ Journaling  
> ✅ Backward Compatibility

---

# Table of Contents

1. What is ext4?
2. Evolution of Linux Filesystems
3. Why ext4 Exists
4. ext4 Architecture
5. ext4 Storage Layout
6. Superblocks
7. Block Groups
8. Inodes
9. Data Blocks
10. Journaling
11. Extents
12. Delayed Allocation
13. Multiblock Allocation
14. Directory Indexing
15. ext4 Features
16. Mount Options
17. Performance Characteristics
18. Common Commands
19. Real World Usage
20. Troubleshooting
21. Interview Questions

---

# What is ext4?

ext4 stands for:

```text
Fourth Extended Filesystem
```

It is the successor to:

```text
ext
 ↓
ext2
 ↓
ext3
 ↓
ext4
```

---

# Evolution of Linux Filesystems

```text
1992
│
├── ext
│
├── ext2
│
├── ext3
│
└── ext4
```

---

# Comparison

| Filesystem | Journaling | Extents | Performance |
|------------|------------|----------|------------|
| ext2 | No | No | Medium |
| ext3 | Yes | No | Medium |
| ext4 | Yes | Yes | High |

---

# Why ext4 Exists

Problems with ext3:

```text
Large Files
Large Volumes
Fragmentation
Scalability
```

Needed improvements.

---

ext4 introduced:

```text
Extents
Delayed Allocation
Faster fsck
Larger Filesystems
Better Performance
```

---

# ext4 Architecture

```text
Filesystem
│
├── Superblock
│
├── Block Groups
│
├── Inode Tables
│
├── Bitmaps
│
└── Data Blocks
```

---

# High-Level View

```text
+-------------------+
| Superblock        |
+-------------------+

+-------------------+
| Block Group 0     |
+-------------------+

+-------------------+
| Block Group 1     |
+-------------------+

+-------------------+
| Block Group N     |
+-------------------+
```

---

# ext4 Storage Layout

Visual:

```text
Disk
│
├── Boot Area
│
├── Superblock
│
├── Group Descriptors
│
├── Block Groups
│
└── Journal
```

---

# Superblock

The superblock contains:

```text
Filesystem Size
Block Size
Free Blocks
Free Inodes
Filesystem State
UUID
```

---

# Visual

```text
Superblock
│
├── Block Count
├── Inode Count
├── UUID
├── State
└── Features
```

---

# View Superblock

```bash
sudo dumpe2fs /dev/sda1
```

---

# Block Groups

ext4 divides disk into:

```text
Block Groups
```

to improve locality.

---

# Visual

```text
Filesystem
│
├── Group 0
├── Group 1
├── Group 2
└── Group 3
```

---

Each group contains:

```text
Block Bitmap
Inode Bitmap
Inode Table
Data Blocks
```

---

# Visual

```text
Block Group
│
├── Block Bitmap
├── Inode Bitmap
├── Inode Table
└── Data Blocks
```

---

# Inodes

Each file gets an inode.

---

Stores:

```text
Permissions
Owner
Size
Timestamps
Block Pointers
```

---

Not stored:

```text
Filename
```

---

# Data Blocks

Actual file content stored here.

---

Visual

```text
File
 │
 ▼
Inode
 │
 ▼
Data Blocks
```

---

# Example

```text
report.txt
      │
      ▼
 inode
      │
      ▼
 block 1
 block 2
 block 3
```

---

# Journaling

One of ext4's most important features.

---

Purpose:

```text
Crash Recovery
Consistency
Metadata Protection
```

---

# Without Journaling

```text
Power Failure
      │
      ▼
Filesystem Corruption
```

---

# With Journaling

```text
Write Intent
      │
      ▼
Journal
      │
      ▼
Actual Write
```

---

# Journal Flow

```text
Application
      │
      ▼
Journal
      │
      ▼
Disk
```

---

# Recovery

After crash:

```text
Read Journal
Replay Operations
Recover Filesystem
```

---

# Extents

Major improvement over ext3.

---

# Traditional Block Mapping

```text
inode
 │
 ├── block 1
 ├── block 2
 ├── block 3
 ├── block 4
 └── block 5
```

Large metadata overhead.

---

# Extent Mapping

```text
inode
 │
 ▼
Extent

Start Block: 100
Length: 500
```

---

Visual

```text
Extent
│
├── Start Block
└── Length
```

---

Benefits:

```text
Less Fragmentation
Less Metadata
Faster Access
```

---

# Delayed Allocation

ext4 delays block assignment.

---

Traditional:

```text
Write
Allocate
Write
Allocate
```

---

ext4:

```text
Write
Write
Write
Allocate Later
```

---

Benefits:

```text
Better Layout
Reduced Fragmentation
Higher Performance
```

---

# Multiblock Allocation

ext4 allocates multiple blocks together.

---

Without:

```text
1 Block
1 Block
1 Block
```

---

With:

```text
100 Blocks
Allocated Together
```

---

Benefits:

```text
Better Sequential I/O
```

---

# Directory Indexing

Large directories become slow.

---

ext4 uses:

```text
HTree
```

(Hashed B-tree)

---

Visual

```text
Directory
│
├── Hash Tree
│
├── Entry A
├── Entry B
└── Entry C
```

---

Benefits:

```text
Fast Lookups
Millions of Files
```

---

# ext4 Features

---

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

## Delayed Allocation

```text
Performance Improvement
```

---

## HTree Directories

```text
Fast Lookups
```

---

## Large Volumes

Supports:

```text
1 EiB Filesystem
16 TiB Files (typical configurations)
```

---

## Backward Compatibility

Can mount:

```text
ext2
ext3
```

filesystems.

---

# Mount Options

View:

```bash
mount | grep ext4
```

---

Common options:

---

## noatime

Disable access time updates.

```bash
mount -o noatime
```

Benefits:

```text
Less Disk Writes
```

---

## relatime

Default on many systems.

---

## data=ordered

Default journaling mode.

---

## errors=remount-ro

Remount read-only on errors.

---

# Performance Characteristics

---

# Strengths

✅ Stable

✅ Mature

✅ Excellent General Purpose FS

✅ Low CPU Usage

✅ Strong Tool Support

---

# Weaknesses

❌ No Native Snapshots

❌ No Built-in Checksums Like Btrfs

❌ Less Advanced Than ZFS

---

# ext4 Write Path

```text
Application
      │
      ▼
Page Cache
      │
      ▼
Journal
      │
      ▼
Data Blocks
      │
      ▼
SSD/HDD
```

---

# ext4 Read Path

```text
Application
      │
      ▼
Page Cache
      │
      ▼
Disk
```

---

# Creating ext4 Filesystem

```bash
mkfs.ext4 /dev/sdb1
```

---

# Check Filesystem

```bash
sudo fsck.ext4 /dev/sdb1
```

---

# View Filesystem Information

```bash
sudo tune2fs -l /dev/sdb1
```

---

# View Block Usage

```bash
df -h
```

---

# View Inode Usage

```bash
df -i
```

---

# Real World Usage

---

# Linux Desktop

```text
Ubuntu
Debian
Mint
```

Default filesystem.

---

# Web Servers

```text
Nginx
Apache
```

Often use ext4.

---

# Database Servers

```text
MySQL
PostgreSQL
```

Common deployment choice.

---

# Virtual Machines

Used extensively by:

```text
KVM
VirtualBox
VMware
```

---

# Docker Hosts

Still commonly deployed on ext4.

---

# Troubleshooting

---

## Check Filesystem

```bash
sudo fsck.ext4 /dev/sda1
```

---

## Show Superblock

```bash
sudo dumpe2fs /dev/sda1
```

---

## Filesystem Information

```bash
sudo tune2fs -l /dev/sda1
```

---

## Inode Usage

```bash
df -i
```

---

## Disk Usage

```bash
df -h
```

---

# ext4 vs XFS

| Feature | ext4 | XFS |
|----------|----------|----------|
| Stability | Excellent | Excellent |
| Snapshots | No | No |
| Journaling | Yes | Yes |
| Large Files | Good | Excellent |
| Recovery Tools | Excellent | Good |
| Small Files | Excellent | Good |
| Default Linux Choice | Yes | Often Servers |

---

# ext4 vs Btrfs

| Feature | ext4 | Btrfs |
|----------|----------|----------|
| Stability | Very High | High |
| Snapshots | No | Yes |
| Checksums | No | Yes |
| Compression | No | Yes |
| Complexity | Low | Higher |
| Performance | Excellent | Good |

---

# Interview Questions

## Beginner

1. What is ext4?
2. What does ext4 stand for?
3. Why was ext4 created?
4. What is journaling?
5. What is an extent?
6. What is a block group?
7. What is a superblock?
8. What command creates ext4?
9. What command checks ext4?
10. What command displays ext4 information?

---

## Intermediate

11. Explain ext4 architecture.
12. Explain journaling modes.
13. Explain delayed allocation.
14. Explain multiblock allocation.
15. Explain extents.
16. Explain HTree indexing.
17. Explain inode storage.
18. Explain block groups.
19. Explain crash recovery.
20. Explain ext4 mount options.

---

## Advanced

21. Explain ext4 journal replay.
22. Explain extent trees.
23. Explain page cache interaction.
24. Explain VFS interaction.
25. Explain ext4 writeback process.
26. Explain metadata journaling.
27. Explain filesystem consistency.
28. Explain superblock redundancy.
29. Explain inode allocation algorithms.
30. Explain ext4 scalability limits.

---

# Summary

```text
ext4
│
├── Superblocks
├── Block Groups
├── Inodes
├── Data Blocks
├── Journaling
├── Extents
├── Delayed Allocation
└── HTree Directories
```

Key Idea:

```text
Application
      │
      ▼
VFS
      │
      ▼
ext4
      │
      ▼
Journal
      │
      ▼
Blocks
      │
      ▼
Disk
```

ext4 remains the most widely deployed Linux filesystem because it offers an excellent balance of:

```text
Performance
Reliability
Simplicity
Compatibility
Maturity
```
