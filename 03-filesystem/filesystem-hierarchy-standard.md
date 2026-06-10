# File System Hierarchy Standard (FHS)

> Understanding the Linux directory structure is essential for Linux administrators, DevOps engineers, developers, security engineers, and system programmers.

The **Filesystem Hierarchy Standard (FHS)** defines how directories should be organized in Linux and UNIX-like operating systems.

---

# Table of Contents

1. What is FHS?
2. Why FHS Exists
3. Linux Directory Tree Overview
4. Root Directory (/)
5. Essential System Directories
6. User Directories
7. Application Directories
8. Temporary Directories
9. Virtual Directories
10. Mount Points
11. Runtime Directories
12. Directory Relationships
13. Real World Examples
14. Best Practices
15. Common Mistakes
16. Interview Questions

---

# What is FHS?

FHS (Filesystem Hierarchy Standard) is a specification that defines:

- Directory names
- Directory purposes
- File locations
- Configuration storage
- Application storage
- User storage

This ensures consistency across Linux distributions.

---

## Without FHS

Every Linux distribution could use:

```text
/settings
/config
/programs
/users
```

making system administration difficult.

---

## With FHS

Every Linux distribution follows a predictable structure:

```text
/
├── etc
├── home
├── usr
├── var
├── tmp
└── boot
```

Administrators instantly know where things belong.

---

# Why FHS Exists

FHS provides:

✅ Consistency

✅ Portability

✅ Easier Administration

✅ Better Security

✅ Easier Troubleshooting

✅ Software Compatibility

---

# Linux Directory Tree Overview

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── lib64
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

# Visual: Linux File System Hierarchy

```text
                         /
                         │
     ┌───────────────────┼───────────────────┐
     │                   │                   │
     ▼                   ▼                   ▼
   System              Users             Virtual
Directories         Directories         Directories
     │                   │                   │
     ▼                   ▼                   ▼
 /bin                /home              /proc
 /etc                /root              /sys
 /usr                                   /dev
 /var
```

---

# Root Directory (/)

The root directory is the top-level directory.

Everything starts from:

```text
/
```

---

## Visual

```text
                /
                │
       ┌────────┼────────┐
       │        │        │
      etc      home     usr
```

---

## Example Paths

```bash
/etc/passwd

/home/user/file.txt

/usr/bin/python3
```

All paths begin at root.

---

# Essential System Directories

---

# /bin

## Purpose

Contains essential user commands.

Required during:

- Boot process
- Recovery mode
- Single-user mode

---

## Examples

```bash
/bin/ls
/bin/cp
/bin/mv
/bin/cat
/bin/bash
```

---

## Visual

```text
/bin
├── ls
├── cp
├── mv
├── cat
└── bash
```

---

# /sbin

## Purpose

Contains system administration commands.

Typically used by root.

---

## Examples

```bash
/sbin/fsck
/sbin/reboot
/sbin/ip
/sbin/shutdown
```

---

## Visual

```text
/sbin
├── fsck
├── reboot
├── ip
└── shutdown
```

---

# /boot

## Purpose

Contains files required for system startup.

---

## Examples

```text
Kernel Image
Bootloader Files
Initial RAM Disk
```

---

## Visual

```text
/boot
├── vmlinuz
├── initrd.img
├── grub
└── System.map
```

---

# Boot Process Visual

```text
Power On
    │
    ▼
BIOS / UEFI
    │
    ▼
GRUB
    │
    ▼
Kernel (/boot)
    │
    ▼
System Startup
```

---

# /etc

## Purpose

Stores system configuration files.

---

## Important Files

```text
/etc/passwd
/etc/group
/etc/fstab
/etc/hosts
/etc/ssh/
```

---

## Visual

```text
/etc
├── passwd
├── group
├── hosts
├── fstab
└── ssh
```

---

## Rule

```text
Configuration Only
```

No executable programs should be stored here.

---

# /lib and /lib64

## Purpose

Contain shared libraries needed by:

```text
/bin
/sbin
```

programs.

---

## Visual

```text
/bin/bash
     │
     ▼
/lib/libc.so
     │
     ▼
Kernel Services
```

---

# User Directories

---

# /home

## Purpose

Stores regular user data.

---

## Visual

```text
/home
├── alice
├── bob
├── john
└── developer
```

---

## Example

```text
/home/alice/Documents
/home/alice/Downloads
/home/alice/Pictures
```

---

## Ownership

Each user owns their own directory.

---

# /root

## Purpose

Home directory of root user.

---

## Difference

```text
/home/user
```

for normal users

vs

```text
/root
```

for administrator

---

## Visual

```text
/
├── home
│   ├── alice
│   └── bob
│
└── root
```

---

# Application Directories

---

# /usr

## Purpose

Contains user applications and software.

---

## Visual

```text
/usr
├── bin
├── sbin
├── lib
├── share
└── local
```

---

## Subdirectories

### /usr/bin

User programs.

```bash
python3
gcc
git
vim
```

---

### /usr/lib

Libraries.

---

### /usr/share

Architecture-independent data.

---

### /usr/local

Locally installed software.

---

## Visual

```text
/usr
│
├── bin
├── lib
├── share
└── local
     ├── bin
     └── lib
```

---

# /opt

## Purpose

Optional third-party software.

---

## Examples

```text
Google Chrome
Custom Enterprise Apps
Oracle Products
```

---

## Visual

```text
/opt
├── chrome
├── oracle
└── custom-app
```

---

# /srv

## Purpose

Stores data served by system services.

---

## Examples

```text
Websites
FTP Data
Application Data
```

---

## Visual

```text
/srv
├── www
├── ftp
└── api
```

---

# Variable Data Directories

---

# /var

## Purpose

Stores changing data.

---

## Visual

```text
/var
├── log
├── cache
├── spool
├── mail
└── lib
```

---

# /var/log

System logs.

---

## Examples

```bash
/var/log/syslog
/var/log/auth.log
```

---

# Logging Flow

```text
Application
     │
     ▼
Log Message
     │
     ▼
/var/log
     │
     ▼
Administrator
```

---

# Temporary Directories

---

# /tmp

Temporary files.

---

## Characteristics

```text
Short-lived
Automatically cleaned
World Writable
```

---

## Visual

```text
/tmp
├── temp1
├── temp2
└── session-data
```

---

# Virtual Directories

Virtual directories do not store data on disk.

They expose kernel information.

---

# /proc

Process Information Filesystem

---

## Visual

```text
/proc
├── cpuinfo
├── meminfo
├── uptime
└── PIDs
```

---

### Example

```bash
cat /proc/cpuinfo
```

---

# /sys

Hardware Information Filesystem

---

## Visual

```text
/sys
├── class
├── devices
├── kernel
└── module
```

---

### Example

```bash
ls /sys/class/net
```

---

# /dev

Device Filesystem

---

## Everything is a File

Linux represents hardware devices as files.

---

## Visual

```text
/dev
├── sda
├── sdb
├── tty
├── null
└── random
```

---

# Device Communication

```text
Application
     │
     ▼
/dev/sda
     │
     ▼
Kernel Driver
     │
     ▼
SSD/HDD
```

---

# Mount Directories

---

# /mnt

Temporary mount point.

---

## Example

```bash
mount /dev/sdb1 /mnt
```

---

# /media

Automatic mount point.

Used by:

```text
USB Drives
DVDs
External Disks
```

---

## Visual

```text
/media
└── user
    └── USB_DRIVE
```

---

# Runtime Directories

---

# /run

Stores runtime system information.

Exists in memory.

---

## Examples

```text
PID Files
Sockets
Lock Files
```

---

## Visual

```text
/run
├── systemd
├── user
└── lock
```

---

# Directory Relationship Diagram

```text
                           /
                           │
 ┌───────────────┬─────────┴───────────┬──────────────┐
 │               │                     │              │
 ▼               ▼                     ▼              ▼
System        Users                Applications   Runtime
 │               │                     │              │
 ▼               ▼                     ▼              ▼
etc           home                  usr           run
boot          root                  opt
bin                                 srv
sbin
lib
```

---

# Real World Examples

## Web Server

```text
Website Code:
/srv/www

Logs:
/var/log

Configuration:
/etc/nginx
```

---

## Database Server

```text
Config:
/etc/mysql

Database Files:
/var/lib/mysql

Logs:
/var/log/mysql
```

---

## Docker Host

```text
Images:
/var/lib/docker

Configuration:
/etc/docker
```

---

# Best Practices

### Store user data in:

```text
/home
```

---

### Store logs in:

```text
/var/log
```

---

### Store configs in:

```text
/etc
```

---

### Install local software in:

```text
/usr/local
```

---

### Mount external devices in:

```text
/media
```

or

```text
/mnt
```

---

# Common Mistakes

❌ Saving application data inside `/etc`

❌ Storing logs in `/home`

❌ Installing software directly in `/bin`

❌ Deleting files from `/boot`

❌ Modifying `/proc`

❌ Modifying `/sys` without understanding effects

---

# FHS Learning Flow

```text
Root (/)
    │
    ▼
System Directories
    │
    ▼
User Directories
    │
    ▼
Applications
    │
    ▼
Logs & Runtime Data
    │
    ▼
Virtual Filesystems
    │
    ▼
Storage & Devices
```

---

# Interview Questions

## Beginner

1. What is FHS?
2. Why is FHS important?
3. What is root directory?
4. Difference between `/home` and `/root`?
5. What is stored in `/etc`?
6. What is `/usr`?
7. What is `/var`?
8. What is `/tmp`?
9. What is `/proc`?
10. What is `/sys`?

---

## Intermediate

11. Explain Linux directory hierarchy.
12. Difference between `/bin` and `/usr/bin`?
13. Difference between `/mnt` and `/media`?
14. What belongs in `/opt`?
15. Explain `/run`.
16. Explain `/srv`.
17. Explain `/dev`.
18. Why is `/proc` virtual?
19. Why is `/sys` virtual?
20. Explain mount points.

---

## Advanced

21. Explain FHS compliance.
22. Explain runtime filesystem hierarchy.
23. Explain device file architecture.
24. Explain procfs internals.
25. Explain sysfs internals.
26. Explain VFS interaction with FHS.
27. Explain boot directory architecture.
28. Explain shared library organization.
29. Explain Linux filesystem namespaces.
30. Explain directory layout optimization.

---

# Summary

```text
/
│
├── System
│   ├── /bin
│   ├── /sbin
│   ├── /boot
│   ├── /etc
│   └── /lib
│
├── Users
│   ├── /home
│   └── /root
│
├── Applications
│   ├── /usr
│   ├── /opt
│   └── /srv
│
├── Variable Data
│   ├── /var
│   └── /tmp
│
└── Virtual Filesystems
    ├── /proc
    ├── /sys
    └── /dev
```

After mastering FHS, you'll immediately know:

- Where Linux stores configuration files
- Where logs are stored
- Where applications live
- Where user files are stored
- How Linux organizes the entire operating system
- How administrators troubleshoot systems efficiently
