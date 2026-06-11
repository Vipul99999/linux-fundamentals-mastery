# Mount Points in Linux

> One of Linux's most powerful filesystem concepts is that **everything appears under a single directory tree**.
>
> Unlike Windows:
>
> ```text
> C:\
> D:\
> E:\
> ```
>
> Linux presents all storage devices, partitions, network filesystems, virtual filesystems, and removable media under a single root:
>
> ```text
> /
> ```
>
> This is made possible through **mount points**.

---

# Table of Contents

1. What is a Mount Point?
2. Why Mount Points Exist
3. Linux Single Directory Tree
4. Mount Point Architecture
5. Mounting Process
6. Filesystems and Mount Points
7. Root Filesystem
8. Common Mount Points
9. Viewing Mounts
10. Mounting Filesystems
11. Unmounting Filesystems
12. Mount Tables
13. /etc/fstab
14. Virtual Filesystem Mounts
15. Bind Mounts
16. Mount Namespaces
17. Container Mounts
18. OverlayFS and Mounts
19. Troubleshooting
20. Interview Questions

---

# What is a Mount Point?

A mount point is:

```text
A Directory
      │
      ▼
Where A Filesystem Is Attached
```

---

# Visual

Before Mount:

```text
/
│
└── data
```

---

After Mount:

```text
/
│
└── data
     │
     ▼
Filesystem
```

---

# Simple Definition

```text
Mount Point
      =
Directory Used To Access
Another Filesystem
```

---

# Why Mount Points Exist

Linux follows:

```text
One Unified Directory Tree
```

Instead of:

```text
Disk A
Disk B
Disk C
```

Linux presents:

```text
/
│
├── home
├── var
├── data
└── backup
```

Each may belong to different filesystems.

---

# Windows vs Linux

Windows:

```text
C:
D:
E:
```

---

Linux:

```text
/
│
├── home
├── data
├── backup
└── media
```

---

# Visual Comparison

Windows

```text
C:\Users
D:\Movies
E:\Backups
```

---

Linux

```text
/
│
├── home
├── movies
└── backups
```

---

# Linux Single Directory Tree

Everything begins from:

```text
/
```

called:

```text
Root Directory
```

---

Visual

```text
/
│
├── home
├── var
├── usr
├── proc
├── sys
└── dev
```

---

# Mount Point Architecture

```text
Directory
     │
     ▼
Mount Point
     │
     ▼
Filesystem
     │
     ▼
Storage Device
```

---

Example

```text
/home
      │
      ▼
ext4 Filesystem
      │
      ▼
/dev/sda2
```

---

# Mounting Process

Suppose:

```text
/dev/sdb1
```

contains:

```text
ext4
```

filesystem.

---

Command:

```bash
mount /dev/sdb1 /data
```

---

Result:

```text
/data
     │
     ▼
Filesystem Contents
```

---

# Visual

Before:

```text
/data
```

(empty directory)

---

After:

```text
/data
│
├── file1
├── file2
└── file3
```

coming from:

```text
/dev/sdb1
```

---

# Filesystems and Mount Points

Relationship:

```text
Filesystem
      │
      ▼
Mounted
      │
      ▼
Directory
```

---

Example

```text
/dev/sda1 → /
/dev/sda2 → /home
/dev/sdb1 → /data
```

---

Visual

```text
Disk Partitions
│
├── sda1
├── sda2
└── sdb1
```

Mounted as:

```text
/
├── home
└── data
```

---

# Root Filesystem

Most important mount.

---

Visual

```text
/
```

---

Without it:

```text
Linux Cannot Boot
```

---

Usually:

```text
/dev/sda1
```

or

```text
/dev/nvme0n1p1
```

---

Mounted as:

```text
/
```

---

# Common Mount Points

---

## Root

```text
/
```

Main filesystem.

---

## Home

```text
/home
```

User files.

---

## Boot

```text
/boot
```

Kernel and bootloader files.

---

## EFI

```text
/boot/efi
```

UEFI firmware files.

---

## Temporary

```text
/tmp
```

Temporary data.

---

## Removable Media

```text
/media
```

USB drives.

---

## Manual Mounts

```text
/mnt
```

Temporary administrator mounts.

---

## ProcFS

```text
/proc
```

Virtual filesystem.

---

## SysFS

```text
/sys
```

Virtual filesystem.

---

## Device Files

```text
/dev
```

Device filesystem.

---

# Visual

```text
/
│
├── home
├── boot
├── proc
├── sys
├── dev
├── media
└── mnt
```

---

# Viewing Mounts

---

## mount

```bash
mount
```

---

Example

```text
/dev/sda1 on /
proc on /proc
sysfs on /sys
```

---

## findmnt

```bash
findmnt
```

---

Tree view:

```text
/
├── /proc
├── /sys
└── /home
```

---

## df

```bash
df -h
```

Shows:

```text
Filesystem
Size
Usage
Mount Point
```

---

# Mounting Filesystems

---

## Manual Mount

```bash
sudo mount /dev/sdb1 /mnt
```

---

## Specify Type

```bash
sudo mount -t ext4 /dev/sdb1 /mnt
```

---

## Read Only

```bash
sudo mount -o ro /dev/sdb1 /mnt
```

---

Visual

```text
Device
   │
   ▼
mount
   │
   ▼
Mount Point
```

---

# Unmounting

Detach filesystem.

---

Command

```bash
umount /mnt
```

---

Visual

```text
Before

/mnt
 │
 ▼
Filesystem
```

---

```text
After

/mnt
```

(empty)

---

# Why Unmount?

Ensures:

```text
All Data Written
No Corruption
```

---

# Mount Tables

Linux tracks mounts internally.

---

Kernel Mount Table

```text
Current Mounts
```

---

View:

```bash
cat /proc/mounts
```

---

Example

```text
/dev/sda1 /
proc /proc
sysfs /sys
```

---

# /etc/fstab

Permanent mount configuration.

---

File:

```text
/etc/fstab
```

---

Example

```text
UUID=1234 / ext4 defaults 0 1
UUID=5678 /home ext4 defaults 0 2
```

---

Visual

```text
Boot
 │
 ▼
Read fstab
 │
 ▼
Mount Filesystems
```

---

# View fstab

```bash
cat /etc/fstab
```

---

# Virtual Filesystem Mounts

Not all mounts use disks.

---

Example:

```text
/proc
/sys
/dev
```

---

Visual

```text
Kernel Data
      │
      ▼
Virtual Filesystem
      │
      ▼
Mount Point
```

---

# Example

```text
proc
 │
 ▼
/proc
```

---

```text
sysfs
 │
 ▼
/sys
```

---

# Bind Mounts

A special mount type.

---

Purpose:

```text
Same Directory
Visible Elsewhere
```

---

Example

```bash
mount --bind /home/user /data/user
```

---

Visual

```text
/home/user
      │
      ▼
Bind Mount
      │
      ▼
/data/user
```

---

Benefits:

```text
Containers
Chroots
Development
```

---

# Mount Namespaces

Used by:

```text
Docker
Podman
Kubernetes
```

---

Each container may have:

```text
Independent Mount View
```

---

Visual

```text
Host
 │
 ├── Namespace A
 │
 └── Namespace B
```

---

Container A sees:

```text
/
├── app
└── data
```

---

Container B sees:

```text
/
├── logs
└── config
```

---

# Container Mounts

Containers rely heavily on:

```text
Bind Mounts
OverlayFS
Mount Namespaces
```

---

Visual

```text
Container
     │
     ▼
Mount Namespace
     │
     ▼
Virtual Filesystem View
```

---

# OverlayFS and Mounts

OverlayFS combines:

```text
Lower Layer
Upper Layer
```

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

Used extensively by:

```text
Docker
```

---

# Troubleshooting

---

## View Mounts

```bash
mount
```

---

## Better Tree View

```bash
findmnt
```

---

## Filesystem Usage

```bash
df -h
```

---

## Mounted Block Devices

```bash
lsblk
```

---

## Open Files Preventing Unmount

```bash
lsof /mnt
```

---

## Force Unmount

```bash
umount -l /mnt
```

---

# Common Mistakes

❌ Thinking mount creates filesystem

✔ It only attaches filesystem

---

❌ Forgetting to unmount USB drives

✔ Risk of corruption

---

❌ Editing fstab incorrectly

✔ Can prevent boot

---

❌ Assuming all mounts are disks

✔ Many are virtual filesystems

---

# Complete Mount Architecture

```text
Application
      │
      ▼
Path
      │
      ▼
Mount Point
      │
      ▼
Filesystem
      │
      ▼
Storage Device
```

---

# Interview Questions

## Beginner

1. What is a mount point?
2. Why does Linux use mount points?
3. What is the root filesystem?
4. What command mounts a filesystem?
5. What command unmounts a filesystem?
6. What is /mnt used for?
7. What is /media used for?
8. What is /proc?
9. What is /sys?
10. How do you view mounted filesystems?

---

## Intermediate

11. Explain filesystem mounting.
12. Explain the Linux directory tree.
13. Explain bind mounts.
14. Explain virtual filesystem mounts.
15. Explain /etc/fstab.
16. Explain mount options.
17. Explain mount propagation.
18. Explain unmount failures.
19. Explain filesystem switching during path traversal.
20. Explain mount tables.

---

## Advanced

21. Explain VFS mount handling.
22. Explain mount namespaces.
23. Explain OverlayFS.
24. Explain Docker mount architecture.
25. Explain kernel mount structures.
26. Explain mount propagation modes.
27. Explain lazy unmount.
28. Explain filesystem crossing.
29. Explain mount resolution during path lookup.
30. Explain container filesystem isolation.

---

# Summary

```text
Mount Point
      │
      ▼
Directory
      │
      ▼
Filesystem
      │
      ▼
Storage Device
```

Key Idea:

```text
Linux
     ≠
Drive Letters

Linux
     =
Single Directory Tree
     +
Mount Points
```

Everything you access in Linux ultimately comes through:

```text
Path
  │
  ▼
Mount Point
  │
  ▼
Filesystem
  │
  ▼
Storage
```

Understanding mount points is essential before learning:

* Storage Management
* LVM
* Containers
* OverlayFS
* Namespaces
* Kubernetes
* Docker
* Linux Boot Process
* Advanced Filesystem Internals

```
```
