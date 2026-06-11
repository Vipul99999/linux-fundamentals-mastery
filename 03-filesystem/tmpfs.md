# tmpfs Filesystem in Linux

> `tmpfs` is a memory-based filesystem that stores files primarily in RAM instead of on disk.
>
> It is one of the fastest filesystems in Linux because data is accessed directly from memory.
>
> Common uses:
>
> * Temporary files
> * Runtime system data
> * Shared memory
> * Containers
> * High-speed caching
>
> Unlike traditional filesystems such as ext4 or XFS, tmpfs does not require a storage device.

---

# Table of Contents

1. What is tmpfs?
2. Why tmpfs Exists
3. tmpfs Architecture
4. How tmpfs Works
5. RAM vs Disk Storage
6. tmpfs vs ramfs
7. tmpfs vs ext4
8. Common tmpfs Mounts
9. Memory Allocation
10. Swap Interaction
11. Mounting tmpfs
12. Shared Memory
13. Containers and tmpfs
14. Performance Characteristics
15. Security Considerations
16. Troubleshooting
17. Interview Questions

---

# What is tmpfs?

tmpfs is a:

```text
Memory-Based Filesystem
```

that stores files in:

```text
RAM
```

and optionally:

```text
Swap Space
```

---

# Visual

```text
Application
      │
      ▼
tmpfs
      │
      ▼
RAM
      │
      ▼
Swap (Optional)
```

---

# Why tmpfs Exists

Traditional filesystems:

```text
ext4
XFS
Btrfs
```

store data on:

```text
SSD
HDD
NVMe
```

---

Accessing storage devices is slower than memory.

---

tmpfs provides:

```text
Extremely Fast Storage
```

for temporary data.

---

# Visual

```text
Disk Storage
     │
     ▼
Milliseconds
```

---

```text
RAM Storage
     │
     ▼
Nanoseconds
```

---

# tmpfs Architecture

```text
Application
      │
      ▼
VFS
      │
      ▼
tmpfs
      │
      ▼
RAM Pages
```

---

# How tmpfs Works

When an application writes:

```bash
echo hello > file.txt
```

inside tmpfs:

```text
Data
 │
 ▼
Memory Pages
```

are allocated.

---

No disk access occurs.

---

# Visual

```text
Application
      │
      ▼
Write File
      │
      ▼
tmpfs
      │
      ▼
RAM
```

---

# RAM vs Disk Storage

Traditional Filesystem

```text
Application
      │
      ▼
Filesystem
      │
      ▼
SSD/HDD
```

---

tmpfs

```text
Application
      │
      ▼
tmpfs
      │
      ▼
RAM
```

---

# Performance Difference

Typical:

| Storage     | Speed          |
| ----------- | -------------- |
| HDD         | Slow           |
| SSD         | Fast           |
| NVMe        | Very Fast      |
| tmpfs (RAM) | Extremely Fast |

---

# Important Property

tmpfs is:

```text
Volatile
```

---

Meaning:

```text
Reboot
   │
   ▼
Data Lost
```

---

# Visual

```text
Power Off
     │
     ▼
RAM Cleared
     │
     ▼
tmpfs Empty
```

---

# tmpfs vs ramfs

Linux provides:

```text
tmpfs
ramfs
```

---

# ramfs

Older filesystem.

---

Characteristics:

```text
RAM Only
No Size Limits
Dangerous
```

---

Visual

```text
ramfs
 │
 ▼
Unlimited Growth
```

---

Problem:

```text
Can Consume All RAM
```

---

# tmpfs

Characteristics:

```text
Size Limits
Swap Support
Safe
```

---

Visual

```text
tmpfs
 │
 ▼
Controlled Memory Usage
```

---

# Comparison

| Feature     | tmpfs | ramfs |
| ----------- | ----- | ----- |
| Uses RAM    | Yes   | Yes   |
| Uses Swap   | Yes   | No    |
| Size Limits | Yes   | No    |
| Safe        | Yes   | No    |

---

# tmpfs vs ext4

| Feature         | tmpfs          | ext4 |
| --------------- | -------------- | ---- |
| Stored In       | RAM            | Disk |
| Persistence     | No             | Yes  |
| Speed           | Extremely Fast | Fast |
| Survives Reboot | No             | Yes  |

---

# Common tmpfs Mounts

Linux uses tmpfs extensively.

---

# /tmp

Some distributions mount:

```text
/tmp
```

as tmpfs.

---

Stores:

```text
Temporary Files
```

---

# /run

Runtime system data.

---

Visual

```text
/run
│
├── PID Files
├── Sockets
└── Runtime State
```

---

# /dev/shm

Shared memory filesystem.

---

Visual

```text
/dev/shm
      │
      ▼
Interprocess Communication
```

---

# Viewing tmpfs Mounts

```bash
mount | grep tmpfs
```

---

Example

```text
tmpfs on /run
tmpfs on /dev/shm
```

---

# Memory Allocation

tmpfs allocates memory dynamically.

---

Visual

```text
Empty tmpfs
      │
      ▼
0 MB Used
```

---

After writing:

```text
File Created
      │
      ▼
Memory Allocated
```

---

# Dynamic Growth

tmpfs grows as needed.

---

Visual

```text
10 MB File
      │
      ▼
10 MB RAM Used
```

---

```text
100 MB File
      │
      ▼
100 MB RAM Used
```

---

# Swap Interaction

One major difference:

```text
tmpfs
      │
      ▼
Can Use Swap
```

---

Visual

```text
RAM Full
    │
    ▼
Move Pages
    │
    ▼
Swap
```

---

Benefits:

```text
Reduced Memory Pressure
```

---

# Mounting tmpfs

---

## Basic Mount

```bash
mount -t tmpfs tmpfs /mnt/tmp
```

---

## Specify Size

```bash
mount -t tmpfs \
-o size=1G \
tmpfs /mnt/tmp
```

---

Visual

```text
tmpfs
 │
 ▼
Maximum 1 GB
```

---

# View Usage

```bash
df -h
```

---

Example

```text
Filesystem Size Used Avail Mounted
tmpfs      1G   0M   1G /mnt/tmp
```

---

# Permanent Mount

Add to:

```text
/etc/fstab
```

---

Example

```text
tmpfs /tmp tmpfs defaults,size=2G 0 0
```

---

# Shared Memory

Linux processes can communicate through:

```text
/dev/shm
```

---

Visual

```text
Process A
      │
      ▼
Shared Memory
      │
      ▼
Process B
```

---

# Example

Applications:

```text
Databases
Browsers
Web Servers
```

often use shared memory.

---

# Containers and tmpfs

Containers use tmpfs heavily.

---

# Docker

Example:

```bash
docker run \
--tmpfs /cache
```

---

Visual

```text
Container
     │
     ▼
tmpfs
     │
     ▼
RAM
```

---

# Kubernetes

Uses:

```text
emptyDir
```

with:

```text
medium: Memory
```

---

Visual

```text
Pod
 │
 ▼
Memory Volume
 │
 ▼
tmpfs
```

---

# Performance Characteristics

---

# Strengths

✅ Extremely Fast

✅ No Disk I/O

✅ Dynamic Allocation

✅ Swap Support

✅ Ideal For Temporary Data

---

# Weaknesses

❌ Volatile

❌ Consumes RAM

❌ Not Suitable For Permanent Storage

---

# Security Considerations

---

# Temporary Secrets

tmpfs is often used for:

```text
Keys
Tokens
Certificates
```

---

Reason:

```text
Not Written To Disk
```

---

# Permissions

Normal Linux permissions apply.

---

Example:

```bash
chmod
chown
```

work normally.

---

# Troubleshooting

---

## Show tmpfs Mounts

```bash
mount | grep tmpfs
```

---

## Show Usage

```bash
df -h
```

---

## Show Memory Usage

```bash
free -h
```

---

## Show Mounted Filesystems

```bash
findmnt
```

---

## View Shared Memory

```bash
df -h /dev/shm
```

---

# Common Misconceptions

❌ tmpfs is stored on disk

✔ Stored in RAM

---

❌ tmpfs survives reboot

✔ Data is lost after reboot

---

❌ tmpfs always consumes maximum memory

✔ Uses memory only when needed

---

❌ tmpfs cannot swap

✔ tmpfs can use swap space

---

# Real World Usage

---

# Web Servers

Cache files.

---

# Databases

Temporary tables.

---

# Containers

Ephemeral storage.

---

# Browsers

Temporary session data.

---

# Systemd

Uses:

```text
/run
```

heavily.

---

# Complete tmpfs Architecture

```text
Application
      │
      ▼
VFS
      │
      ▼
tmpfs
      │
      ▼
RAM Pages
      │
      ▼
Swap (Optional)
```

---

# Interview Questions

## Beginner

1. What is tmpfs?
2. Where is tmpfs stored?
3. Does tmpfs survive reboot?
4. What is `/dev/shm`?
5. What is `/run`?

---

## Intermediate

6. Explain tmpfs architecture.
7. Explain swap interaction.
8. Explain dynamic memory allocation.
9. Explain tmpfs vs ext4.
10. Explain tmpfs vs ramfs.

---

## Advanced

11. Explain tmpfs implementation.
12. Explain VFS interaction.
13. Explain page cache relationship.
14. Explain tmpfs memory accounting.
15. Explain tmpfs usage in containers.
16. Explain Kubernetes memory volumes.
17. Explain Docker tmpfs mounts.
18. Explain shared memory internals.
19. Explain tmpfs scalability.
20. Explain tmpfs security considerations.

---

# Summary

```text
tmpfs
│
├── RAM-Based
├── Fast
├── Dynamic Allocation
├── Swap Support
├── Volatile
└── Temporary Storage
```

Core Idea:

```text
Disk Filesystem
      │
      ▼
Persistent Storage

tmpfs
      │
      ▼
Memory Storage
```

Remember:

```text
tmpfs
     =
Fast
     +
Temporary
     +
Memory-Based
```

Understanding tmpfs is essential before learning:

* Linux Memory Management
* Page Cache
* Containers
* Docker
* Kubernetes
* Shared Memory
* Systemd Internals
* Performance Optimization
* Linux Kernel Memory Subsystems

```
```
