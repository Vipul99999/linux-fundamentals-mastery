# Btrfs Filesystem in Linux

> Btrfs (B-Tree File System) is a modern Linux filesystem designed to provide advanced features such as:
>
> * Snapshots
> * Copy-on-Write (CoW)
> * Checksums
> * Compression
> * RAID Support
> * Self-Healing
> * Subvolumes
>
> Btrfs aims to be a next-generation filesystem capable of replacing traditional filesystems such as ext4.

---

# Table of Contents

1. What is Btrfs?
2. Why Btrfs Exists
3. Btrfs Architecture
4. Core Concepts
5. Copy-on-Write (CoW)
6. B-Trees
7. Checksums
8. Subvolumes
9. Snapshots
10. Compression
11. RAID Support
12. Data Integrity
13. Self-Healing
14. Storage Pools
15. Send/Receive
16. Scrubbing
17. Balancing
18. Performance Characteristics
19. Common Commands
20. Real World Usage
21. Troubleshooting
22. Btrfs vs ext4
23. Btrfs vs XFS
24. Interview Questions

---

# What is Btrfs?

Btrfs stands for:

```text
B-Tree File System
```

Created to solve limitations of traditional filesystems.

Goals:

```text
Scalability
Data Integrity
Snapshots
Storage Management
Reliability
```

---

# Why Btrfs Exists

Traditional filesystems suffer from:

```text
No snapshots
Limited integrity checking
No built-in RAID
No self-healing
No storage pooling
```

---

Btrfs introduces:

```text
Copy-on-Write
Checksums
Snapshots
Subvolumes
Compression
Built-in RAID
```

---

# High-Level Architecture

```text
Filesystem
│
├── B-Trees
├── Subvolumes
├── Snapshots
├── Checksums
├── RAID
└── Storage Pool
```

---

# Btrfs Storage Model

Traditional:

```text
Filesystem
      │
      ▼
Single Device
```

---

Btrfs:

```text
Filesystem
      │
      ▼
Storage Pool
      │
      ├── Disk A
      ├── Disk B
      └── Disk C
```

---

# Core Concepts

Btrfs is built around:

```text
Copy-on-Write
B-Trees
Checksums
Subvolumes
Snapshots
```

---

# Copy-on-Write (CoW)

Most important Btrfs feature.

---

# Traditional Write

```text
Old Block
    │
    ▼
Overwrite
```

Risk:

```text
Crash During Write
```

can corrupt data.

---

# CoW Write

```text
Old Block
    │
    ▼
Create New Block
    │
    ▼
Update Metadata
```

---

Visual:

```text
Old Data
    │
    ▼
New Copy
    │
    ▼
Modify Copy
```

---

Benefits:

```text
Snapshots
Integrity
Crash Safety
```

---

# B-Trees

Everything in Btrfs is stored in trees.

---

Visual

```text
Root Tree
│
├── Chunk Tree
├── Extent Tree
├── Device Tree
├── Checksum Tree
└── Filesystem Tree
```

---

Benefits:

```text
Fast Lookup
Scalable Metadata
Efficient Storage
```

---

# Major Trees

---

## Root Tree

Tracks subvolumes.

---

## Extent Tree

Tracks storage allocation.

---

## Checksum Tree

Stores checksums.

---

## Chunk Tree

Tracks physical storage.

---

## Device Tree

Tracks devices.

---

# Checksums

Every data block has a checksum.

---

Visual

```text
Data Block
     │
     ▼
Checksum
```

---

Purpose:

```text
Detect Corruption
```

---

Example

```text
Stored Data
Stored Checksum
```

---

When reading:

```text
Recalculate Checksum
       │
       ▼
Compare
```

---

If mismatch:

```text
Corruption Detected
```

---

# Subvolumes

Unique Btrfs feature.

---

A subvolume acts like:

```text
Independent Filesystem
```

inside the same filesystem.

---

Visual

```text
Filesystem
│
├── Root
├── Home
├── Backups
└── Containers
```

---

Create:

```bash
btrfs subvolume create home
```

---

Benefits:

```text
Isolation
Snapshots
Management
```

---

# Snapshots

One of the most powerful Btrfs features.

---

Snapshot:

```text
Read-Only
or
Read-Write
```

copy of filesystem state.

---

Visual

```text
Subvolume
     │
     ▼
Snapshot
```

---

Because of CoW:

```text
No Immediate Data Copy
```

---

Snapshot Creation

```bash
btrfs subvolume snapshot \
/home /snapshots/home
```

---

Visual

```text
Original
     │
     ▼
Snapshot
```

---

Initially:

```text
Shared Blocks
```

---

Modified data:

```text
New Blocks Created
```

---

# Compression

Built-in filesystem compression.

---

Supported:

```text
zlib
lzo
zstd
```

---

Visual

```text
Data
 │
 ▼
Compression
 │
 ▼
Disk
```

---

Benefits:

```text
Less Storage
Less I/O
```

---

Enable:

```bash
mount -o compress=zstd
```

---

# RAID Support

Built directly into filesystem.

---

Supported:

```text
RAID0
RAID1
RAID10
RAID5
RAID6
```

---

Visual

```text
Disk A
Disk B
Disk C
Disk D
```

managed directly by Btrfs.

---

# Data Integrity

One of Btrfs's biggest strengths.

---

Traditional Filesystem

```text
Disk Corruption
      │
      ▼
Unknown
```

---

Btrfs

```text
Checksum Verification
      │
      ▼
Corruption Detection
```

---

# Self-Healing

When redundancy exists:

```text
RAID1
RAID10
```

Btrfs can repair corrupted blocks automatically.

---

Visual

```text
Bad Block
     │
     ▼
Healthy Copy
     │
     ▼
Repair
```

---

# Storage Pools

Traditional:

```text
Filesystem
      │
      ▼
Single Partition
```

---

Btrfs:

```text
Filesystem
      │
      ▼
Multiple Devices
```

---

Add Device

```bash
btrfs device add /dev/sdb /mnt/data
```

---

# Send / Receive

Unique replication feature.

---

Purpose:

```text
Incremental Backup
```

---

Send:

```bash
btrfs send snapshot
```

---

Receive:

```bash
btrfs receive backup
```

---

Visual

```text
Snapshot A
      │
      ▼
Send Stream
      │
      ▼
Remote Backup
```

---

# Scrubbing

Verifies checksums.

---

Command:

```bash
btrfs scrub start /
```

---

Process:

```text
Read Data
Verify Checksum
Repair If Possible
```

---

Visual

```text
Data Block
      │
      ▼
Checksum Check
      │
      ▼
Repair
```

---

# Balancing

Redistributes data.

---

Purpose:

```text
Storage Optimization
```

---

Command:

```bash
btrfs balance start /
```

---

Visual

```text
Uneven Storage
       │
       ▼
Balance
       │
       ▼
Even Distribution
```

---

# Performance Characteristics

---

# Strengths

✅ Snapshots

✅ Compression

✅ Checksums

✅ RAID

✅ Self-Healing

✅ Subvolumes

---

# Weaknesses

❌ Higher metadata overhead

❌ More RAM usage

❌ CoW may affect databases

❌ More complex than ext4

---

# CoW and Databases

Databases often perform:

```text
Frequent Updates
```

---

CoW causes:

```text
Extra Writes
```

---

Solutions:

```bash
chattr +C database_dir
```

Disable CoW.

---

# Creating Btrfs Filesystem

```bash
mkfs.btrfs /dev/sdb1
```

---

# Mounting

```bash
mount -t btrfs /dev/sdb1 /mnt
```

---

# Filesystem Information

```bash
btrfs filesystem show
```

---

# Show Usage

```bash
btrfs filesystem usage /
```

---

# Create Subvolume

```bash
btrfs subvolume create data
```

---

# Create Snapshot

```bash
btrfs subvolume snapshot \
data backup
```

---

# Run Scrub

```bash
btrfs scrub start /
```

---

# Run Balance

```bash
btrfs balance start /
```

---

# Real World Usage

---

# Fedora

Default filesystem.

---

# openSUSE

Default filesystem.

---

# Snapper

Uses snapshots for rollback.

---

# Containers

Useful for:

```text
Docker
Podman
Container Storage
```

---

# Backup Systems

Popular because of:

```text
Snapshots
Send/Receive
```

---

# Btrfs vs ext4

| Feature     | Btrfs    | ext4      |
| ----------- | -------- | --------- |
| Snapshots   | Yes      | No        |
| Checksums   | Yes      | No        |
| Compression | Yes      | No        |
| RAID        | Built-In | External  |
| CoW         | Yes      | No        |
| Complexity  | Higher   | Lower     |
| Stability   | High     | Very High |

---

# Btrfs vs XFS

| Feature     | Btrfs     | XFS           |
| ----------- | --------- | ------------- |
| Snapshots   | Yes       | No            |
| Checksums   | Yes       | Metadata Only |
| Compression | Yes       | No            |
| RAID        | Built-In  | External      |
| Scalability | Excellent | Excellent     |
| Complexity  | Higher    | Medium        |

---

# Troubleshooting

---

## Show Filesystem

```bash
btrfs filesystem show
```

---

## Show Usage

```bash
btrfs filesystem usage /
```

---

## Scrub Filesystem

```bash
btrfs scrub start /
```

---

## Balance Filesystem

```bash
btrfs balance start /
```

---

## Check Device Status

```bash
btrfs device stats /
```

---

## Show Subvolumes

```bash
btrfs subvolume list /
```

---

# Interview Questions

## Beginner

1. What is Btrfs?
2. What does Btrfs stand for?
3. What is Copy-on-Write?
4. What is a snapshot?
5. What is a subvolume?
6. What is filesystem compression?
7. What is a checksum?
8. What is scrubbing?
9. What is balancing?
10. Why is Btrfs considered modern?

---

## Intermediate

11. Explain CoW architecture.
12. Explain Btrfs trees.
13. Explain checksum verification.
14. Explain snapshots.
15. Explain subvolumes.
16. Explain RAID support.
17. Explain send/receive.
18. Explain balancing.
19. Explain self-healing.
20. Explain compression.

---

## Advanced

21. Explain extent allocation.
22. Explain chunk trees.
23. Explain root trees.
24. Explain checksum trees.
25. Explain CoW performance impact.
26. Explain snapshot internals.
27. Explain RAID implementation.
28. Explain scrub internals.
29. Explain VFS interaction.
30. Explain Btrfs scalability architecture.

---

# Summary

```text
Btrfs
│
├── Copy-on-Write
├── Snapshots
├── Subvolumes
├── Checksums
├── Compression
├── RAID
├── Self-Healing
├── Scrubbing
└── Send/Receive
```

Core Idea:

```text
Application
      │
      ▼
VFS
      │
      ▼
Btrfs
      │
      ├── CoW
      ├── Checksums
      ├── Snapshots
      ├── RAID
      └── Compression
      │
      ▼
Storage Devices
```

Btrfs is designed to be an all-in-one modern filesystem providing:

```text
Data Integrity
Snapshots
Storage Management
Scalability
Reliability
Advanced Features
```
