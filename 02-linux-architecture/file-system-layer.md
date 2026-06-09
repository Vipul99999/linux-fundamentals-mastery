# File System Layer

The File System Layer is responsible for organizing, storing, retrieving, and managing data on storage devices. Linux provides a unified file abstraction through the Virtual File System (VFS), allowing multiple file systems to operate under a common interface.

---

# Table of Contents

1. What is a File System?
2. Why File Systems Exist
3. File System Architecture
4. Virtual File System (VFS)
5. Linux Directory Hierarchy
6. Inodes
7. File Types
8. File Operations
9. File Descriptors
10. Mounting
11. Journaling
12. Common Linux File Systems
13. Storage Access Flow
14. Real-World Examples
15. Interview Questions

---

# What is a File System?

A File System organizes data on storage devices.

Without a filesystem:

```text
Disk
│
├── Raw Data
├── Raw Data
├── Raw Data
└── Raw Data
```

No structure exists.

With a filesystem:

```text
/
├── home
├── etc
├── var
├── usr
└── tmp
```

---

# Why File Systems Exist

File systems provide:

* Organization
* Security
* Permissions
* Fast Retrieval
* Data Integrity

---

# File System Architecture

```text
+------------------------+
| User Applications      |
+------------------------+
            │
            ▼
+------------------------+
| System Calls           |
+------------------------+
            │
            ▼
+------------------------+
| VFS (Virtual FS)       |
+------------------------+
            │
            ▼
+------------------------+
| EXT4 / XFS / Btrfs     |
+------------------------+
            │
            ▼
+------------------------+
| Storage Device         |
+------------------------+
```

---

# Virtual File System (VFS)

VFS provides a common interface.

```text
Application
      │
      ▼
VFS
      │
 ┌────┼────┐
 ▼    ▼    ▼
EXT4 XFS BTRFS
```

Benefits:

* Unified API
* Multiple filesystem support

---

# Linux Directory Hierarchy

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
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

# Important Directories

| Directory | Purpose                |
| --------- | ---------------------- |
| /bin      | Essential Commands     |
| /etc      | Configuration Files    |
| /home     | User Files             |
| /dev      | Device Files           |
| /proc     | Process Information    |
| /tmp      | Temporary Files        |
| /var      | Logs and Variable Data |

---

# Linux Storage Hierarchy

```text
Disk
 │
 ▼
Partition
 │
 ▼
Filesystem
 │
 ▼
Directories
 │
 ▼
Files
```

---

# Inodes

An inode stores metadata.

```text
+----------------+
| Inode          |
+----------------+
| Owner          |
| Permissions    |
| Size           |
| Timestamps     |
| Data Pointers  |
+----------------+
```

---

# File Structure

```text
Filename
    │
    ▼
Inode
    │
    ▼
Disk Blocks
```

---

# Inode Example

```text
report.pdf
     │
     ▼
 Inode #1234
     │
     ▼
Data Blocks
```

---

# File Types

```text
Linux Files
│
├── Regular File
├── Directory
├── Character Device
├── Block Device
├── Symbolic Link
├── Socket
└── FIFO
```

---

# File Permissions

```text
-rwxr-xr--
```

Visualization:

```text
Owner   Group   Others
rwx     r-x     r--
```

Meaning:

```text
r = Read
w = Write
x = Execute
```

---

# File Operations

Common operations:

```text
open()
read()
write()
close()
unlink()
rename()
```

---

# File Access Flow

```text
Application
      │
      ▼
open()
      │
      ▼
VFS
      │
      ▼
Filesystem
      │
      ▼
Storage Driver
      │
      ▼
SSD/HDD
```

---

# File Descriptors

Every opened file receives an FD.

```text
0 → stdin
1 → stdout
2 → stderr
3 → file.txt
```

Visualization:

```text
Application
      │
      ▼
FD = 3
      │
      ▼
file.txt
```

---

# Mounting

Linux attaches filesystems into a single hierarchy.

```text
Filesystem
      │
      ▼
Mount Point
      │
      ▼
Directory Tree
```

Example:

```bash
mount /dev/sdb1 /mnt/data
```

---

# Mount Architecture

```text
Root Filesystem
       │
       ├── /home
       │
       ├── /mnt/data
       │
       └── /media/usb
```

---

# Journaling

Journaling improves recovery after crashes.

Without journaling:

```text
Power Failure
      │
      ▼
Filesystem Corruption
```

With journaling:

```text
Operation
      │
      ▼
Journal
      │
      ▼
Disk
```

Recovery becomes easier.

---

# Common Linux Filesystems

```text
Linux Filesystems
│
├── EXT4
├── XFS
├── Btrfs
├── FAT32
├── NTFS
├── ZFS
└── NFS
```

---

# EXT4 Architecture

```text
EXT4
│
├── Superblock
├── Inodes
├── Journals
└── Data Blocks
```

---

# Storage Read Flow

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
EXT4
      │
      ▼
Block Driver
      │
      ▼
SSD
```

---

# Storage Write Flow

```text
Application
      │
      ▼
write()
      │
      ▼
Page Cache
      │
      ▼
Filesystem
      │
      ▼
Storage Device
```

---

# Real-World Example

Opening:

```bash
cat notes.txt
```

Flow:

```text
cat
 │
 ▼
open()
 │
 ▼
VFS
 │
 ▼
EXT4
 │
 ▼
SSD
 │
 ▼
Data Returned
```

---

# Linux Filesystem Commands

View mounted filesystems:

```bash
mount
```

Disk usage:

```bash
df -h
```

Directory size:

```bash
du -sh
```

Filesystem info:

```bash
lsblk
```

---

# Production Use Cases

```text
Databases
Web Servers
Cloud Storage
Containers
Virtual Machines
NAS Systems
File Servers
Backup Systems
```

---

# Interview Questions

1. What is a filesystem?
2. What is VFS?
3. What is an inode?
4. What is journaling?
5. What is mounting?
6. Difference between EXT4 and XFS?
7. What is a file descriptor?
8. Explain file access flow.
9. What is the purpose of /proc?
10. How does Linux organize storage?

---

# Summary

The Linux File System Layer provides a structured way to store and access data through VFS, inodes, filesystems, directories, and storage devices. It abstracts storage complexity while ensuring security, performance, scalability, and reliability for applications ranging from personal desktops to large-scale cloud infrastructure.
