# Inodes in Linux

> If the Linux filesystem were a library:
>
> - Filename = Book Title
> - Directory = Catalog
> - Inode = Complete Book Information Card
> - Data Blocks = Actual Pages of the Book
>
> Understanding inodes is essential because Linux does **not store files using filenames internally**. Linux stores files using **inode numbers**.

---

# Table of Contents

1. What is an Inode?
2. Why Inodes Exist
3. File Storage Architecture
4. Inode Structure
5. What an Inode Stores
6. What an Inode Does NOT Store
7. File Creation Process
8. How Linux Finds a File
9. Inode Number
10. Data Blocks
11. Direct and Indirect Pointers
12. Hard Links and Inodes
13. Symbolic Links and Inodes
14. Inode Tables
15. Inode Exhaustion
16. Viewing Inodes
17. Inode Cache
18. VFS and Inodes
19. Real World Examples
20. Troubleshooting
21. Interview Questions

---

# What is an Inode?

An inode (Index Node) is a data structure that stores metadata about a file.

Every file and directory in Linux has:

```text
Inode
```

except a few special virtual filesystem objects.

---

# Visual Overview

```text
Filename
   │
   ▼
report.txt
   │
   ▼
Inode #45231
   │
   ├── Owner
   ├── Permissions
   ├── Size
   ├── Timestamps
   └── Block Addresses
           │
           ▼
      Actual Data
```

---

# Why Inodes Exist

Linux separates:

```text
File Name
```

from

```text
File Metadata
```

and

```text
File Content
```

This provides:

- Faster file management
- Multiple names for same file
- Efficient filesystem operations
- Better storage organization

---

# File Storage Architecture

```text
+------------------+
| Directory Entry  |
+------------------+
        │
        ▼
+------------------+
| Inode Number     |
+------------------+
        │
        ▼
+------------------+
| Inode Structure  |
+------------------+
        │
        ▼
+------------------+
| Data Blocks      |
+------------------+
```

---

# Real Storage Layout

```text
Filesystem
│
├── Boot Block
│
├── Superblock
│
├── Inode Table
│
└── Data Blocks
```

---

# Visual

```text
┌───────────────────────┐
│ Superblock            │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Inode Table           │
│ inode 1               │
│ inode 2               │
│ inode 3               │
│ inode 4               │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Data Blocks           │
└───────────────────────┘
```

---

# What an Inode Stores

Each inode contains metadata.

---

## File Type

Examples:

```text
Regular File
Directory
Symbolic Link
Socket
FIFO
Device File
```

---

## Permissions

```bash
-rwxr-xr--
```

stored in inode.

---

## Owner UID

```text
User ID
```

---

## Group ID

```text
Group Owner
```

---

## File Size

Example:

```text
5 KB
500 MB
10 GB
```

---

## Timestamps

Linux stores:

| Timestamp | Meaning |
|------------|------------|
| atime | Last access |
| mtime | Last modification |
| ctime | Metadata change |

---

## Link Count

Tracks number of hard links.

---

## Data Block Addresses

Pointers to actual data.

---

# What an Inode Does NOT Store

One of the most important interview questions.

---

## Filename is NOT Stored

Linux stores:

```text
Filename
→ Inode Number
```

inside directories.

---

## Visual

```text
Directory
│
├── report.txt → inode 100
├── notes.txt  → inode 200
└── data.txt   → inode 300
```

---

## Why?

Allows:

```text
Multiple filenames
       │
       ▼
Same inode
```

which is the basis of hard links.

---

# File Creation Process

Suppose:

```bash
touch report.txt
```

---

## Step 1

Kernel allocates inode.

```text
inode 4500
```

---

## Step 2

Directory entry created.

```text
report.txt → inode 4500
```

---

## Step 3

Data blocks allocated when data is written.

---

## Visual

```text
touch report.txt

     │

     ▼

Directory Entry
     │
     ▼
report.txt
     │
     ▼
inode 4500
     │
     ▼
Data Blocks
```

---

# How Linux Finds a File

Example:

```bash
cat report.txt
```

---

## Lookup Process

```text
Directory
      │
      ▼
Find Filename
      │
      ▼
Get Inode Number
      │
      ▼
Read Inode
      │
      ▼
Locate Data Blocks
      │
      ▼
Read Data
```

---

# Complete Path Resolution Example

```bash
/home/user/report.txt
```

Kernel performs:

```text
/
│
├── home
│
├── user
│
└── report.txt
```

---

## Visual

```text
Root Directory
      │
      ▼
home
      │
      ▼
user
      │
      ▼
report.txt
      │
      ▼
inode
      │
      ▼
data blocks
```

---

# Inode Number

Each inode has a unique identifier.

Example:

```bash
ls -i
```

Output:

```text
45231 report.txt
```

where:

```text
45231
```

is inode number.

---

# View Inode Information

```bash
stat report.txt
```

---

Example:

```text
Inode: 45231
Size: 4096
Links: 1
UID: 1000
GID: 1000
```

---

# Data Blocks

The inode does not store actual data.

Instead:

```text
inode
   │
   ▼
Pointers
   │
   ▼
Disk Blocks
```

---

## Visual

```text
inode
│
├── Block 101
├── Block 102
├── Block 103
└── Block 104
```

Actual file contents are stored there.

---

# Direct and Indirect Pointers

Large files need many blocks.

Linux uses:

```text
Direct Pointers
Indirect Pointers
Double Indirect
Triple Indirect
```

---

# Direct Pointers

```text
inode
│
├── block 1
├── block 2
├── block 3
└── block 4
```

Fast access.

---

# Single Indirect

```text
inode
   │
   ▼
Indirect Block
   │
   ├── block 100
   ├── block 101
   └── block 102
```

---

# Double Indirect

```text
inode
     │
     ▼
Indirect Block
     │
     ▼
Indirect Block
     │
     ▼
Data Blocks
```

---

# Triple Indirect

Used for huge files.

```text
inode
  │
  ▼
Indirect
  │
  ▼
Indirect
  │
  ▼
Indirect
  │
  ▼
Data
```

---

# Hard Links and Inodes

Hard links share same inode.

---

## Example

```bash
ln report.txt backup.txt
```

---

## Visual

```text
report.txt
      │
      ▼
    inode 500
      ▲
      │
backup.txt
```

---

## Check

```bash
ls -li
```

Both files show same inode.

---

# Link Count

```bash
stat report.txt
```

Output:

```text
Links: 2
```

because:

```text
report.txt
backup.txt
```

point to same inode.

---

# Symbolic Links and Inodes

Symlink has its own inode.

---

## Visual

```text
shortcut
    │
    ▼
inode 700
    │
    ▼
stores path:
report.txt
```

---

## Result

```text
report.txt → inode 500

shortcut → inode 700
```

Different inode.

---

# Inode Table

Every filesystem reserves inode storage.

---

## Visual

```text
Filesystem
│
├── Superblock
│
├── Inode Table
│    ├── inode 1
│    ├── inode 2
│    ├── inode 3
│    └── inode 4
│
└── Data Blocks
```

---

# Inode Exhaustion

A filesystem may have:

```text
Free Disk Space
```

but still fail.

---

## Example

```text
Disk Free = 20 GB

Inodes Free = 0
```

Cannot create files.

---

# Check Inode Usage

```bash
df -i
```

Output:

```text
Filesystem
Inodes
IUsed
IFree
```

---

# Visual

```text
Available Storage
        │
        ▼
20 GB Free

BUT

Available Inodes
        │
        ▼
0 Free

Result:
No More Files Can Be Created
```

---

# Inode Cache

Reading inode from disk repeatedly is slow.

Linux uses:

```text
inode cache
```

inside memory.

---

# Visual

```text
Application
      │
      ▼
inode cache
      │
      ▼
disk
```

Most requests hit cache first.

---

# VFS and Inodes

The Virtual File System uses inode objects.

---

## Architecture

```text
Application
      │
      ▼
System Call
      │
      ▼
VFS
      │
      ▼
Inode Object
      │
      ▼
Filesystem Driver
```

---

# Real World Examples

---

## Web Server

```text
/var/www
```

Millions of files.

Each file:

```text
Own inode
```

---

## Git Repository

```text
.git
```

Contains thousands of inode entries.

---

## Docker

OverlayFS manages huge inode structures.

---

## Database Server

Database files:

```text
Own inode
Own blocks
Own metadata
```

---

# Troubleshooting

## Show Inode Number

```bash
ls -i
```

---

## Detailed Inode Info

```bash
stat file.txt
```

---

## Check Filesystem Inodes

```bash
df -i
```

---

## Find Files By Inode

```bash
find . -inum 45231
```

---

## Show Hard Links

```bash
ls -li
```

---

# Common Misconceptions

❌ Inode stores filename

✔ No, directory stores filename

---

❌ File content stored in inode

✔ Data stored in blocks

---

❌ Hard links create copies

✔ Same inode shared

---

❌ Symlink shares inode

✔ Symlink has different inode

---

# Interview Questions

## Beginner

1. What is an inode?
2. Why does Linux use inodes?
3. What does an inode store?
4. What does an inode not store?
5. How to view inode number?
6. What is inode metadata?
7. What is inode table?
8. What is link count?
9. What are data blocks?
10. What is inode exhaustion?

---

## Intermediate

11. Explain file lookup using inode.
12. Explain inode table architecture.
13. Explain inode cache.
14. Explain inode allocation.
15. Explain hard links using inodes.
16. Explain symlink inode structure.
17. Explain path resolution.
18. Explain VFS inode objects.
19. Explain block pointers.
20. Explain inode lifecycle.

---

## Advanced

21. Explain ext4 inode structure.
22. Explain inode caching mechanisms.
23. Explain inode locking.
24. Explain dentry-inode relationship.
25. Explain inode reference counting.
26. Explain inode journaling interactions.
27. Explain inode allocation algorithms.
28. Explain inode scalability issues.
29. Explain inode recovery after corruption.
30. Explain VFS inode abstraction.

---

# Summary

```text
Filename
    │
    ▼
Directory Entry
    │
    ▼
Inode Number
    │
    ▼
Inode Metadata
    │
    ▼
Data Block Addresses
    │
    ▼
Actual File Data
```

Remember:

```text
Filename ≠ File

Filename → Inode

Inode → Metadata

Inode → Data Blocks

Data Blocks → Actual Content
```

This concept is the foundation for:

- Hard Links
- Symbolic Links
- Path Resolution
- VFS
- Filesystem Internals
- Storage Architecture
- Linux System Administration
