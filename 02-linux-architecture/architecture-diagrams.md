# Linux Architecture Diagrams

This document contains visual representations of the major Linux architecture components discussed throughout this module. These diagrams are designed to help learners understand how Linux subsystems interact with each other and how data flows through the operating system.

---

# Table of Contents

1. Complete Linux Architecture
2. User Space vs Kernel Space
3. Shell to Kernel Flow
4. System Call Flow
5. Process Lifecycle
6. Process Hierarchy
7. Scheduler Architecture
8. Memory Layout
9. Virtual Memory Translation
10. Paging Architecture
11. Page Table Translation
12. File System Architecture
13. VFS Architecture
14. File Read Flow
15. File Write Flow
16. Device Driver Architecture
17. Interrupt Handling
18. Networking Architecture
19. Boot Process
20. Container Architecture
21. Docker Architecture
22. Linux Security Architecture
23. Everything Together Diagram

---

# Complete Linux Architecture

```text
+-------------------------------------------------------+
|                   User Applications                   |
|-------------------------------------------------------|
| Chrome | VSCode | Docker | Python | Nginx | MySQL    |
+-------------------------------------------------------+
|                  Shell / GUI Layer                    |
|-------------------------------------------------------|
| Bash | Zsh | Fish | GNOME | KDE                       |
+-------------------------------------------------------+
|                  System Call Interface                |
+-------------------------------------------------------+
|                    Linux Kernel                       |
|-------------------------------------------------------|
| Process Manager                                       |
| Memory Manager                                        |
| Scheduler                                             |
| Virtual File System                                   |
| Device Drivers                                        |
| Networking Stack                                      |
| Security Modules                                      |
+-------------------------------------------------------+
|                     Hardware                          |
|-------------------------------------------------------|
| CPU | RAM | SSD | GPU | NIC | USB | Keyboard | Mouse |
+-------------------------------------------------------+
```

---

# User Space vs Kernel Space

```text
+------------------------------------------------+
|                 USER SPACE                     |
|------------------------------------------------|
| Applications                                   |
| Libraries                                      |
| Shells                                         |
| Daemons                                        |
+------------------------------------------------+

                System Calls

+------------------------------------------------+
|                KERNEL SPACE                    |
|------------------------------------------------|
| Scheduler                                      |
| Memory Manager                                 |
| VFS                                             |
| Drivers                                        |
| Networking                                     |
| Security                                       |
+------------------------------------------------+
```

---

# Shell to Kernel Flow

```text
User
 │
 ▼
Shell (bash)
 │
 ▼
Command Parsing
 │
 ▼
System Call
 │
 ▼
Kernel
 │
 ▼
Hardware
```

Example:

```text
ls -la
   │
   ▼
bash
   │
   ▼
execve()
   │
   ▼
Kernel
   │
   ▼
Filesystem
```

---

# System Call Flow

```text
Application
      │
      ▼
glibc Library
      │
      ▼
System Call
      │
      ▼
CPU Mode Switch
      │
      ▼
Kernel Handler
      │
      ▼
Kernel Service
      │
      ▼
Return Result
      │
      ▼
Application
```

---

# Process Lifecycle

```text
       New
        │
        ▼
      Ready
        │
        ▼
     Running
      /   \
     /     \
    ▼       ▼
Waiting   Ready
    │
    ▼
 Running
    │
    ▼
Terminated
```

---

# Process Hierarchy

```text
systemd (PID 1)
│
├── sshd
│    └── bash
│          └── python
│
├── nginx
│
├── mysql
│
└── docker
     ├── container1
     ├── container2
     └── container3
```

---

# Scheduler Architecture

```text
+-------------------+
| Ready Queue       |
+-------------------+
          │
          ▼
+-------------------+
| Linux Scheduler   |
+-------------------+
          │
          ▼
+-------------------+
| CPU Core          |
+-------------------+
          │
          ▼
+-------------------+
| Running Process   |
+-------------------+
```

---

# CPU Time Sharing

```text
Timeline →
--------------------------------------------------

Process A : ███

Process B :    ███

Process C :       ███

Process D :          ███
```

Appears simultaneous to users.

---

# Process Creation Flow

```text
Parent Process
        │
        ▼
      fork()
        │
        ▼
 ┌──────────────┐
 │              │
 ▼              ▼
Parent       Child
                 │
                 ▼
              exec()
                 │
                 ▼
            New Program
```

---

# Process Context Switching

```text
Process A Running
       │
       ▼
Save CPU State
       │
       ▼
Load Process B State
       │
       ▼
Process B Running
```

---

# Process Memory Layout

```text
High Address
+-----------------------+
| Kernel Space          |
+-----------------------+
| Stack                 |
+-----------------------+
| Shared Libraries      |
+-----------------------+
| Memory Mapped Files   |
+-----------------------+
| Heap                  |
+-----------------------+
| BSS                   |
+-----------------------+
| Data Segment          |
+-----------------------+
| Text Segment          |
+-----------------------+
Low Address
```

---

# Heap and Stack Growth

```text
+--------------------+
| Stack              |
|        ▼           |
|                    |
|                    |
|                    |
|        ▲           |
| Heap               |
+--------------------+
```

---

# Virtual Memory Translation

```text
Application
      │
      ▼
Virtual Address
      │
      ▼
MMU
      │
      ▼
Page Table
      │
      ▼
Physical Address
      │
      ▼
RAM
```

---

# Paging Architecture

```text
Virtual Memory

+--------+
| Page 1 |
+--------+
| Page 2 |
+--------+
| Page 3 |
+--------+

      │

      ▼

Physical Memory

+---------+
| Frame 5 |
+---------+
| Frame 1 |
+---------+
| Frame 9 |
+---------+
```

---

# TLB Architecture

```text
CPU
 │
 ▼
TLB Cache
 │
 ├── Hit
 │      │
 │      ▼
 │    RAM
 │
 └── Miss
        │
        ▼
   Page Table
```

---

# Demand Paging

```text
Program Start
      │
      ▼
Load First Page
      │
      ▼
Page Access
      │
      ▼
Page Fault
      │
      ▼
Load Required Page
```

---

# Copy-On-Write (COW)

```text
Parent Process
      │
      ▼
      fork()
      │
      ▼

Shared Pages
     ▲ ▲
     │ │
 Parent Child

Write Occurs
     │
     ▼

Copy Created
```

---

# File System Architecture

```text
+------------------------+
| User Application       |
+------------------------+
           │
           ▼
+------------------------+
| System Calls           |
+------------------------+
           │
           ▼
+------------------------+
| VFS                    |
+------------------------+
           │
   ┌───────┼────────┐
   ▼       ▼        ▼
 EXT4     XFS     Btrfs
           │
           ▼
+------------------------+
| Block Layer            |
+------------------------+
           │
           ▼
+------------------------+
| SSD / HDD / NVMe       |
+------------------------+
```

---

# VFS Architecture

```text
Application
      │
      ▼
VFS
      │
 ┌────┼─────┬─────┐
 ▼    ▼     ▼     ▼
EXT4 XFS  Btrfs  NFS
```

---

# File Read Flow

```text
Application
      │
      ▼
read()
      │
      ▼
Page Cache
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
Storage Device
```

---

# File Write Flow

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
Journal
      │
      ▼
SSD / HDD
```

---

# Inode Architecture

```text
Filename
    │
    ▼
 Inode
    │
    ├── Owner
    ├── Permissions
    ├── Size
    ├── Timestamps
    └── Block Pointers
           │
           ▼
       Data Blocks
```

---

# Device Driver Architecture

```text
Application
      │
      ▼
System Call
      │
      ▼
Kernel
      │
      ▼
Driver
      │
      ▼
Hardware
```

---

# Interrupt Handling

```text
Keyboard
    │
    ▼
Interrupt
    │
    ▼
CPU
    │
    ▼
Interrupt Handler
    │
    ▼
Driver
    │
    ▼
Application
```

---

# DMA (Direct Memory Access)

Without DMA:

```text
Device
   │
   ▼
CPU
   │
   ▼
RAM
```

With DMA:

```text
Device
   │
   ▼
RAM
```

---

# Linux Networking Architecture

```text
Application
      │
      ▼
Socket API
      │
      ▼
TCP / UDP
      │
      ▼
IP Layer
      │
      ▼
Network Driver
      │
      ▼
Network Card
      │
      ▼
Internet
```

---

# Packet Flow

```text
Browser
   │
   ▼
Socket
   │
   ▼
TCP
   │
   ▼
IP
   │
   ▼
NIC Driver
   │
   ▼
Ethernet/WiFi
```

---

# Linux Boot Process

```text
Power On
    │
    ▼
BIOS / UEFI
    │
    ▼
Bootloader (GRUB)
    │
    ▼
Linux Kernel
    │
    ▼
Init/Systemd
    │
    ▼
Services
    │
    ▼
Login Screen
```

---

# Docker Architecture

```text
+-----------------------------------+
| Container A                       |
+-----------------------------------+

+-----------------------------------+
| Container B                       |
+-----------------------------------+

+-----------------------------------+
| Container C                       |
+-----------------------------------+

-------------------------------------
Shared Linux Kernel
-------------------------------------

Hardware
```

---

# Container Isolation

```text
Container
│
├── Process Namespace
├── Network Namespace
├── Mount Namespace
├── User Namespace
└── cgroups
```

---

# Linux Security Architecture

```text
Security
│
├── Users
├── Groups
├── Permissions
├── ACLs
├── SELinux
├── AppArmor
├── Namespaces
└── cgroups
```

---

# Permission Model

```text
-rwxr-xr--

Owner    Group   Others
rwx      r-x     r--
```

---

# Complete Linux Internal Flow

Opening a File:

```text
User
 │
 ▼
Shell
 │
 ▼
Application
 │
 ▼
System Call
 │
 ▼
Kernel
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
Driver
 │
 ▼
SSD
 │
 ▼
Data Returned
```

---

# Everything Together

```text
+---------------------------------------------------------+
| Applications                                            |
+---------------------------------------------------------+
| Shells / GUI                                            |
+---------------------------------------------------------+
| System Calls                                            |
+---------------------------------------------------------+
| Process Manager | Memory Manager | VFS | Networking     |
| Drivers | Scheduler | Security | IPC | Caching          |
+---------------------------------------------------------+
| CPU | RAM | SSD | GPU | NIC | USB | Keyboard | Mouse    |
+---------------------------------------------------------+
```

---

# Diagram Navigation Guide

These diagrams correspond to:

```text
kernel.md
shell.md
user-space.md
kernel-space.md
system-calls.md
process-management.md
memory-management.md
device-drivers.md
file-system-layer.md
```

Use this document as a quick visual reference while studying Linux architecture.
