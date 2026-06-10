# Linux File System (VFS, FHS, Inodes, Links & Virtual File Systems)

> "Everything in Linux is a file."
>
> Understanding the Linux File System is one of the most important skills for Linux administrators, developers, DevOps engineers, security engineers, and system programmers.

---

# Table of Contents

1. Introduction
2. What is a File System?
3. Why Linux Uses a Hierarchical File System
4. Linux File System Architecture
5. Virtual File System (VFS)
6. File System Hierarchy Standard (FHS)
7. Understanding Inodes
8. Hard Links
9. Symbolic Links
10. Path Resolution
11. Virtual File Systems
12. Proc File System (/proc)
13. Sys File System (/sys)
14. Common File Systems in Linux
15. File System Operations Flow
16. Storage Stack Architecture
17. Performance Considerations
18. Security Concepts
19. Real World Examples
20. Troubleshooting Guide
21. Learning Roadmap
22. Interview Questions
23. References

---

# 1. Introduction

A Linux File System is responsible for:

- Storing data
- Organizing data
- Retrieving data
- Managing permissions
- Managing metadata
- Managing storage devices

Without a file system:

- No files
- No directories
- No applications
- No operating system functionality

---

# 2. What is a File System?

A file system is a method used by the operating system to organize and store data on storage devices.

Examples:

| Device | File System |
|----------|----------|
| Linux SSD | ext4 |
| Windows SSD | NTFS |
| macOS SSD | APFS |
| USB Drive | FAT32 |
| Large Storage Server | XFS |

---

# Linux View

Linux treats nearly everything as a file:

```text
Regular File
Directory
Device
Socket
Pipe
Process Information
Hardware Information
```

Example:

```bash
/dev/sda
/dev/null
/proc/cpuinfo
/sys/class/net
```

---

# 3. Why Linux Uses a Hierarchical File System

Unlike Windows:

```text
C:\
D:\
E:\
```

Linux has a single tree structure.

```text
/
├── home
├── etc
├── usr
├── var
├── tmp
└── dev
```

Everything begins from:

```text
/
```

called the:

```text
Root Directory
```

---

# 4. Linux File System Architecture

```text
Applications
      │
      ▼
System Calls
(open, read, write)
      │
      ▼
Virtual File System (VFS)
      │
      ▼
Filesystem Drivers
(ext4, xfs, btrfs)
      │
      ▼
Block Layer
      │
      ▼
Storage Device
```

---

# 5. Virtual File System (VFS)

## What is VFS?

VFS is an abstraction layer inside the Linux kernel.

It provides:

```text
Common Interface
```

for all file systems.

---

## Without VFS

Applications would need:

```text
Special code for ext4
Special code for xfs
Special code for btrfs
Special code for NFS
```

Huge complexity.

---

## With VFS

Applications use:

```c
open()
read()
write()
close()
```

Kernel handles everything.

---

### VFS Architecture

```text
Application
     │
     ▼
open()
     │
     ▼
VFS
 ├── ext4
 ├── xfs
 ├── btrfs
 ├── nfs
 └── procfs
```

---

## Core VFS Objects

### Superblock

Represents filesystem.

```text
Filesystem Metadata
```

---

### Inode

Represents file metadata.

---

### Dentry

Directory entry cache.

---

### File Object

Represents opened file.

---

# 6. File System Hierarchy Standard (FHS)

Linux follows FHS.

---

## Complete Hierarchy

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

## Important Directories

### /etc

Configuration files.

```bash
/etc/passwd
/etc/fstab
/etc/ssh
```

---

### /home

User files.

```text
/home/john
/home/admin
```

---

### /var

Variable data.

```text
logs
cache
spool
```

---

### /tmp

Temporary files.

---

### /usr

Applications and libraries.

---

### /boot

Kernel and bootloader.

---

### /dev

Device files.

---

### /proc

Process information.

---

### /sys

Hardware information.

---

# 7. Understanding Inodes

An inode is the heart of Linux file systems.

---

## Inode Stores

```text
Permissions
Owner
Group
Size
Timestamps
Block Locations
File Type
```

---

## Inode Does NOT Store

```text
Filename
```

Important interview question.

---

### Visual

```text
Filename
   │
   ▼
report.txt
   │
   ▼
Inode #5678
   │
   ├── permissions
   ├── owner
   ├── timestamps
   └── disk blocks
```

---

## View Inode

```bash
ls -i file.txt
```

Output:

```text
34567 file.txt
```

---

# 8. Hard Links

Hard link = another filename pointing to same inode.

---

### Visual

```text
file.txt
     │
     ▼
  inode 100
     ▲
     │
backup.txt
```

Both point to same inode.

---

## Create Hard Link

```bash
ln file.txt backup.txt
```

---

## Characteristics

✅ Same inode

✅ Same data

✅ Fast

❌ Cannot cross filesystems

❌ Cannot link directories

---

# 9. Symbolic Links

Soft link = shortcut.

---

### Visual

```text
shortcut.txt
      │
      ▼
   file.txt
      │
      ▼
   inode 100
```

---

## Create Symlink

```bash
ln -s file.txt shortcut.txt
```

---

## Characteristics

✅ Cross filesystem

✅ Link directories

✅ Flexible

❌ Breaks if target removed

---

### Hard Link vs Soft Link

| Feature | Hard | Soft |
|----------|--------|--------|
| Same Inode | Yes | No |
| Cross FS | No | Yes |
| Directory Link | No | Yes |
| Survives Rename | Yes | Usually |
| Broken Link Possible | No | Yes |

---

# 10. Path Resolution

When:

```bash
cat /home/user/file.txt
```

Kernel performs path resolution.

---

### Flow

```text
/
│
├── home
│
├── user
│
└── file.txt
```

Kernel traverses:

```text
root
 → home
 → user
 → file.txt
```

using dentries and inodes.

---

# 11. Virtual File Systems

Virtual filesystems do not store real data on disk.

They expose kernel information.

Examples:

```text
procfs
sysfs
tmpfs
cgroupfs
```

---

### Visual

```text
Kernel Data
      │
      ▼
Virtual Filesystem
      │
      ▼
Appears as files
```

---

# 12. Proc File System (/proc)

Provides process and kernel information.

---

### Example

```bash
cat /proc/cpuinfo
```

---

### Process Directory

```bash
/proc/1234
```

Contains:

```text
status
fd
maps
cmdline
```

---

### Proc Structure

```text
/proc
│
├── cpuinfo
├── meminfo
├── uptime
├── loadavg
└── PID directories
```

---

# 13. Sys File System (/sys)

Provides hardware and driver information.

---

### Example

```bash
/sys/class/net
/sys/block
/sys/devices
```

---

### Architecture

```text
Hardware
    │
    ▼
Kernel
    │
    ▼
sysfs
    │
    ▼
/sys
```

---

# 14. Common Linux File Systems

## ext4

Most common.

Features:

```text
Journaling
Fast
Stable
Widely supported
```

---

## XFS

Best for:

```text
Large files
Large servers
```

---

## Btrfs

Features:

```text
Snapshots
Checksums
Compression
```

---

## ZFS

Features:

```text
RAID
Snapshots
Self-healing
```

---

# 15. File System Operation Flow

Reading a file:

```text
Application
      │
      ▼
read()
      │
      ▼
VFS
      │
      ▼
Filesystem Driver
      │
      ▼
Page Cache
      │
      ▼
Disk
```

---

# 16. Storage Stack Architecture

```text
User Application
       │
       ▼
System Call
       │
       ▼
VFS
       │
       ▼
Filesystem
       │
       ▼
Block Layer
       │
       ▼
I/O Scheduler
       │
       ▼
Device Driver
       │
       ▼
SSD / HDD
```

---

# 17. Performance Considerations

## Page Cache

Frequently used files remain in memory.

```text
RAM
 ↓
Cache Hit
 ↓
No Disk Access
```

---

## Journaling

Protects filesystem consistency.

Example:

```text
Power Failure
      │
      ▼
Journal Recovery
```

---

## SSD Optimization

Use:

```bash
fstrim
```

for TRIM support.

---

# 18. Security Concepts

## File Permissions

```bash
-rwxr-xr--
```

---

## Ownership

```bash
user
group
```

---

## ACL

Advanced permissions.

```bash
getfacl
setfacl
```

---

## Immutable Files

```bash
chattr +i file.txt
```

Cannot be modified.

---

# 19. Real World Examples

## Web Server

```text
/var/www
```

Stores website.

---

## Database Server

```text
/var/lib/mysql
```

Stores database files.

---

## Docker

Uses:

```text
OverlayFS
```

for image layers.

---

## Kubernetes

Uses:

```text
tmpfs
cgroups
procfs
sysfs
```

---

# 20. Troubleshooting Guide

## Disk Usage

```bash
df -h
```

---

## Directory Size

```bash
du -sh *
```

---

## Inode Usage

```bash
df -i
```

---

## Mounted Filesystems

```bash
mount
```

---

## Open Files

```bash
lsof
```

---

# 21. Learning Roadmap

```text
Linux Files
      │
      ▼
Directories
      │
      ▼
Permissions
      │
      ▼
Inodes
      │
      ▼
Hard Links
      │
      ▼
Soft Links
      │
      ▼
VFS
      │
      ▼
ext4/xfs
      │
      ▼
procfs/sysfs
      │
      ▼
Storage Internals
      │
      ▼
Kernel Filesystems
```

---

# 22. Interview Questions

### Beginner

1. What is a file system?
2. What is VFS?
3. What is inode?
4. Difference between hard and soft links?
5. What is FHS?
6. What is root directory?
7. What is journaling?
8. What is ext4?
9. What is procfs?
10. What is sysfs?

### Intermediate

11. Explain VFS architecture.
12. Explain inode structure.
13. Explain path resolution.
14. How does Linux locate a file?
15. What are dentries?
16. Explain page cache.
17. Explain journaling recovery.
18. Explain mount points.
19. Difference between ext4 and XFS?
20. Explain OverlayFS.

### Advanced

21. Explain VFS internals.
22. Explain dentry cache.
23. Explain inode cache.
24. Explain ext4 journaling.
25. Explain writeback caching.
26. Explain page cache architecture.
27. Explain mount namespace.
28. Explain filesystem drivers.
29. Explain OverlayFS internals.
30. Explain filesystem crash recovery.

---

# 23. What You Will Learn in This Module

After completing this folder you will understand:

✅ Linux File System Architecture

✅ VFS Internals

✅ File System Hierarchy Standard

✅ Inodes

✅ Hard Links

✅ Symbolic Links

✅ Path Resolution

✅ ProcFS

✅ SysFS

✅ Virtual File Systems

✅ Storage Stack

✅ Performance Concepts

✅ Real World Linux File System Design

---

# Folder Structure

```text
03-filesystem/
│
├── README.md
├── filesystem-hierarchy-standard.md
├── inodes.md
├── hard-links.md
├── symbolic-links.md
├── path-resolution.md
├── virtual-filesystems.md
├── proc-filesystem.md
├── sys-filesystem.md
├── labs/
└── exercises/
```
