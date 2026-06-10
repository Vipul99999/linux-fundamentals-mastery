# Path Resolution in Linux

> Every time you run:
>
> ```bash
> cat file.txt
> ```
>
> or
>
> ```bash
> cd /home/user/Documents
> ```
>
> Linux performs a process called **Path Resolution** (also called Pathname Resolution or Path Traversal).
>
> Path Resolution is the process of converting a pathname into the actual inode representing a file or directory.

---

# Table of Contents

1. What is Path Resolution?
2. Why Path Resolution Exists
3. Path Components
4. Absolute Paths
5. Relative Paths
6. Current Working Directory
7. Path Resolution Architecture
8. Directory Traversal
9. Dentries and Inodes
10. Symbolic Link Resolution
11. Mount Point Resolution
12. VFS Path Lookup
13. Path Cache (Dentry Cache)
14. Special Path Components
15. Real World Examples
16. Performance Optimization
17. Security Considerations
18. Troubleshooting
19. Interview Questions

---

# What is Path Resolution?

Path Resolution is the process of converting:

```text
/home/user/report.txt
```

into:

```text
inode number
```

that represents the file.

---

## High-Level Flow

```text
Path
  │
  ▼
Directory Traversal
  │
  ▼
Inode Lookup
  │
  ▼
Target File
```

---

# Why Path Resolution Exists

Humans use:

```text
/home/user/file.txt
```

Computers use:

```text
inode numbers
```

Path resolution bridges the gap.

---

# Visual

```text
Human
  │
  ▼
"/home/user/file.txt"
  │
  ▼
Kernel
  │
  ▼
inode 45231
```

---

# Path Components

A path consists of components separated by:

```text
/
```

---

## Example

```bash
/home/user/report.txt
```

Components:

```text
home
user
report.txt
```

---

## Visual

```text
/home/user/report.txt

 │
 ▼

home
 │
 ▼

user
 │
 ▼

report.txt
```

---

# Absolute Paths

Absolute paths start from:

```text
/
```

(root directory)

---

## Example

```bash
/etc/passwd
```

---

## Visual

```text
/
│
└── etc
     │
     └── passwd
```

---

## Characteristics

✅ Independent of current location

✅ Always starts at root

---

# Relative Paths

Relative paths start from:

```text
Current Working Directory
```

---

## Example

Current directory:

```bash
/home/user
```

Command:

```bash
cat report.txt
```

Actual path:

```bash
/home/user/report.txt
```

---

## Visual

```text
Current Directory
      │
      ▼
 /home/user
      │
      ▼
 report.txt
```

---

# Current Working Directory (CWD)

Every process maintains:

```text
Current Working Directory
```

---

## Check

```bash
pwd
```

Example:

```text
/home/user/projects
```

---

# Visual

```text
Process
   │
   ▼
Current Directory
   │
   ▼
/home/user/projects
```

---

# Path Resolution Architecture

---

## High-Level Flow

```text
Application
      │
      ▼
System Call
(open/read/stat)
      │
      ▼
Virtual File System (VFS)
      │
      ▼
Path Resolution
      │
      ▼
Filesystem Driver
      │
      ▼
Inode
```

---

# Example Walkthrough

Suppose:

```bash
cat /home/user/report.txt
```

Linux performs:

---

## Step 1

Start at root.

```text
/
```

---

## Step 2

Find:

```text
home
```

directory entry.

---

## Step 3

Read inode of:

```text
home
```

---

## Step 4

Find:

```text
user
```

inside home.

---

## Step 5

Read inode.

---

## Step 6

Find:

```text
report.txt
```

---

## Step 7

Open target inode.

---

# Visual

```text
/
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
```

---

# Directory Traversal

Directories contain:

```text
filename → inode
```

mappings.

---

## Example

```text
/home

├── user  → inode 200
├── admin → inode 300
└── test  → inode 400
```

---

Linux repeatedly:

```text
Read Directory
Find Name
Get Inode
Move Forward
```

---

# Visual

```text
Directory
    │
    ▼
Find Component
    │
    ▼
Get Inode
    │
    ▼
Next Component
```

---

# Dentries and Inodes

Path resolution relies heavily on:

### Dentry

Directory Entry Cache

---

### Inode

Metadata Object

---

## Relationship

```text
Filename
     │
     ▼
 Dentry
     │
     ▼
 Inode
```

---

# Visual

```text
report.txt
      │
      ▼
 Dentry Cache
      │
      ▼
 Inode
```

---

# Symbolic Link Resolution

Suppose:

```text
shortcut.txt
```

is a symlink.

---

Visual:

```text
shortcut.txt
      │
      ▼
"/home/user/report.txt"
```

---

Kernel process:

```text
Read Symlink
      │
      ▼
Get Stored Path
      │
      ▼
Resolve New Path
      │
      ▼
Find Target
```

---

# Visual

```text
shortcut
    │
    ▼
stored path
    │
    ▼
target file
```

---

# Symlink Chains

Example:

```text
link1 → link2
link2 → link3
link3 → file
```

---

Visual

```text
link1
 │
 ▼
link2
 │
 ▼
link3
 │
 ▼
file
```

---

Kernel follows chain until real file found.

---

# Symlink Loop Detection

Bad example:

```text
link1 → link2
link2 → link1
```

---

Visual

```text
link1
 ▲
 │
 ▼
link2
```

---

Linux detects loop and returns:

```text
ELOOP
```

---

# Special Path Components

---

# Current Directory (.)

```text
.
```

means:

```text
Current Directory
```

---

Example

```bash
./program
```

---

Visual

```text
Current Directory
       │
       ▼
   program
```

---

# Parent Directory (..)

```text
..
```

means:

```text
Parent Directory
```

---

Example

```bash
cd ..
```

---

Visual

```text
/home/user
      │
      ▼
     ..
      │
      ▼
    /home
```

---

# Root Directory (/)

```text
/
```

always represents filesystem root.

---

# Mount Point Resolution

Suppose:

```text
/mnt/usb
```

is mounted.

---

Visual

```text
/
│
└── mnt
      │
      ▼
     usb
      │
      ▼
Another Filesystem
```

---

Kernel switches filesystem context.

---

# Example

```bash
mount /dev/sdb1 /mnt/usb
```

---

Now:

```bash
cd /mnt/usb
```

crosses into another filesystem.

---

# VFS Path Lookup

The Virtual File System provides a generic lookup mechanism.

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
Dentry Lookup
      │
      ▼
Inode Lookup
      │
      ▼
Filesystem Driver
```

---

# Dentry Cache

Repeated path lookups are expensive.

Linux caches results.

---

## Visual

```text
Path Request
      │
      ▼
Dentry Cache
      │
   Hit?
   / \
 Yes  No
  │    │
  ▼    ▼
Return Disk Lookup
```

---

# Example

First access:

```bash
cat report.txt
```

may require disk lookup.

---

Second access:

```bash
cat report.txt
```

may come directly from cache.

---

# Path Resolution Performance

---

## Without Cache

```text
Disk Access
Disk Access
Disk Access
Disk Access
```

Slow.

---

## With Cache

```text
Memory Lookup
```

Fast.

---

# Real World Example

---

## Opening a File

```bash
nano /etc/passwd
```

Path resolution:

```text
/
 │
 ▼
etc
 │
 ▼
passwd
 │
 ▼
inode
```

---

## Running a Program

```bash
/usr/bin/python3
```

Resolution:

```text
/
 │
 ▼
usr
 │
 ▼
bin
 │
 ▼
python3
 │
 ▼
inode
```

---

# Security Considerations

Path resolution is security critical.

---

# Directory Traversal Attacks

Bad input:

```text
../../../etc/passwd
```

---

Visual

```text
Current Directory
      │
      ▼
     ..
      │
      ▼
     ..
      │
      ▼
     ..
      │
      ▼
/etc/passwd
```

---

Applications must validate paths.

---

# Symlink Attacks

Example:

```text
Temporary File
       │
       ▼
Malicious Symlink
```

Can redirect privileged programs.

---

# Path Canonicalization

Converts:

```text
/home/user/../user/file
```

into:

```text
/home/user/file
```

---

# Visual

```text
Input Path
     │
     ▼
Normalize
     │
     ▼
Canonical Path
```

---

# Troubleshooting

---

## Show Current Directory

```bash
pwd
```

---

## Show Real Path

```bash
realpath file.txt
```

---

## Resolve Symlink

```bash
readlink -f link
```

---

## Show Mounts

```bash
mount
```

or

```bash
findmnt
```

---

## Trace File Access

```bash
strace -e openat ls
```

---

# Common Mistakes

❌ Thinking Linux searches entire disk

✔ Linux traverses directories

---

❌ Thinking filename directly identifies file

✔ Inode identifies file

---

❌ Ignoring symlink resolution

✔ Kernel resolves symlinks

---

❌ Forgetting mount points

✔ Path may switch filesystems

---

# Advanced Path Resolution Flow

```text
Application
      │
      ▼
open("/home/user/report.txt")
      │
      ▼
VFS
      │
      ▼
Root Dentry
      │
      ▼
Lookup home
      │
      ▼
Lookup user
      │
      ▼
Lookup report.txt
      │
      ▼
Get Inode
      │
      ▼
Open File
```

---

# Interview Questions

## Beginner

1. What is path resolution?
2. What is an absolute path?
3. What is a relative path?
4. What is the current working directory?
5. What does "." mean?
6. What does ".." mean?
7. How does Linux locate a file?
8. What is an inode?
9. What is a dentry?
10. What is a mount point?

---

## Intermediate

11. Explain path traversal.
12. Explain VFS lookup.
13. Explain symlink resolution.
14. Explain mount point traversal.
15. Explain dentry cache.
16. Explain relative path handling.
17. Explain inode lookup.
18. Explain path normalization.
19. Explain filesystem crossing.
20. Explain lookup failures.

---

## Advanced

21. Explain Linux namei().
22. Explain dentry caching internals.
23. Explain path walk algorithms.
24. Explain RCU path lookup.
25. Explain VFS path resolution.
26. Explain mount namespace impact.
27. Explain symlink security issues.
28. Explain pathname lookup optimization.
29. Explain kernel path traversal implementation.
30. Explain filesystem lookup operations.

---

# Summary

```text
Path
 │
 ▼
Directory Traversal
 │
 ▼
Dentry Lookup
 │
 ▼
Inode Lookup
 │
 ▼
Target File
```

Remember:

```text
Path Resolution
      ≠
Disk Search

Path Resolution
      =
Directory Traversal
      +
Inode Lookup
      +
Symlink Handling
      +
Mount Resolution
```

Understanding Path Resolution is essential before learning:

- Virtual File System (VFS)
- ProcFS
- SysFS
- Mount Namespaces
- Containers
- Linux Security
- Filesystem Internals
- Kernel File Operations
