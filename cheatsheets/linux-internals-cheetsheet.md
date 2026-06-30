# Linux Internals Cheat Sheet

## The Complete Kernel, System Architecture, and Operating System Engineering Reference

---

# Why This Exists

Most people use Linux.

Few understand Linux.

Most engineers know:

```bash
ls
ps
top
systemctl
```

But very few understand:

```text
What happens when a process starts?

How does memory work?

How does the scheduler work?

How does a file get read from disk?

How does networking reach an application?

How do containers actually work?

How does the kernel control everything?
```

Understanding Linux Internals is the difference between:

```text
Linux User
    ↓
Linux Administrator
    ↓
Linux Engineer
    ↓
Systems Engineer
    ↓
Infrastructure Architect
```

This cheat sheet provides a high-speed map of the entire Linux operating system.

---

# The Ultimate Linux Mental Model

Linux is a resource manager.

Everything Linux does revolves around managing:

```text
CPU
Memory
Storage
Network
Devices
Processes
```

---

# Linux Architecture

```mermaid
graph TD

USER["Users"]

USER --> APPS["Applications"]

APPS --> LIBS["Libraries"]

LIBS --> SYSCALLS["System Calls"]

SYSCALLS --> KERNEL["Linux Kernel"]

KERNEL --> CPU["CPU"]
KERNEL --> MEM["Memory"]
KERNEL --> DISK["Storage"]
KERNEL --> NET["Network"]
KERNEL --> DEV["Devices"]
```

---

# Linux In One Picture

```text
+----------------------------------+
|          Applications            |
+----------------------------------+

| Nginx | PostgreSQL | Docker |

+----------------------------------+
|      System Libraries (glibc)    |
+----------------------------------+

+----------------------------------+
|        System Calls API          |
+----------------------------------+

+----------------------------------+
|          Linux Kernel            |
+----------------------------------+

| CPU | Memory | Disk | Network |

+----------------------------------+
|             Hardware             |
+----------------------------------+
```

---

# The Linux Boot Journey

```mermaid
flowchart TD

A[Power On]

A --> B[BIOS/UEFI]

B --> C[Bootloader]

C --> D[Linux Kernel]

D --> E[Initramfs]

E --> F[systemd PID 1]

F --> G[Services]

G --> H[Applications]
```

---

# Linux Kernel

The kernel is the core of Linux.

Responsibilities:

```text
Process Management

Memory Management

Device Management

Filesystem Management

Networking

Security
```

---

# Kernel Space vs User Space

```mermaid
graph TD

USER["User Space"]

USER --> SYSCALL["System Call"]

SYSCALL --> KERNEL["Kernel Space"]
```

---

# User Space

Examples:

```text
Bash
Nginx
PostgreSQL
Python
Docker
Java
```

Cannot directly access hardware.

---

# Kernel Space

Has unrestricted access.

Controls:

```text
CPU
RAM
Disks
NICs
Devices
```

---

# System Calls

Applications communicate with Linux through system calls.

---

# System Call Flow

```mermaid
sequenceDiagram

participant App
participant Kernel
participant Disk

App->>Kernel: open()

Kernel->>Disk: Read File

Disk-->>Kernel: Data

Kernel-->>App: Return Data
```

---

# Common System Calls

| System Call | Purpose            |
| ----------- | ------------------ |
| open()      | Open file          |
| read()      | Read file          |
| write()     | Write file         |
| fork()      | Create process     |
| exec()      | Run program        |
| socket()    | Create socket      |
| connect()   | Network connection |
| mmap()      | Map memory         |

---

# Observe System Calls

```bash
strace ls
```

---

# Process Architecture

A process contains:

```text
Code
Stack
Heap
Threads
File Descriptors
Memory Mappings
```

---

# Process Layout

```mermaid
graph TD

PROCESS["Process"]

PROCESS --> CODE["Code"]

PROCESS --> DATA["Data"]

PROCESS --> HEAP["Heap"]

PROCESS --> STACK["Stack"]
```

---

# Process Creation

Linux uses:

```text
fork()
```

and

```text
exec()
```

---

# Process Creation Flow

```mermaid
graph LR

PARENT["Parent"]

PARENT --> FORK["fork()"]

FORK --> CHILD["Child Process"]

CHILD --> EXEC["exec()"]

EXEC --> PROGRAM["New Program"]
```

---

# Process States

```text
R = Running

S = Sleeping

D = Uninterruptible

T = Stopped

Z = Zombie
```

---

# View Process States

```bash
ps aux
```

---

# Linux Scheduler

Controls CPU access.

---

# Scheduler Goal

Answer:

```text
Who gets CPU next?
```

---

# Scheduling Architecture

```mermaid
graph TD

CPU["CPU"]

CPU --> PROC1["Process A"]

CPU --> PROC2["Process B"]

CPU --> PROC3["Process C"]

SCHED["Scheduler"]

SCHED --> CPU
```

---

# Modern Scheduler

Linux uses:

```text
CFS
Completely Fair Scheduler
```

---

# Memory Architecture

Memory is divided into:

```text
Kernel Memory

User Memory
```

---

# Virtual Memory

Every process believes:

```text
I own all memory.
```

Reality:

```text
Kernel maps virtual memory to physical memory.
```

---

# Virtual Memory Flow

```mermaid
graph LR

PROCESS["Process"]

PROCESS --> VM["Virtual Memory"]

VM --> MMU["MMU"]

MMU --> RAM["Physical RAM"]
```

---

# Memory Layout

```text
+----------------+
| Stack          |
+----------------+
| Heap           |
+----------------+
| Data Segment   |
+----------------+
| Code Segment   |
+----------------+
```

---

# Memory Commands

```bash
free -h

vmstat

top

htop
```

---

# Page Cache

Linux aggressively caches disk data.

---

# Why?

RAM is faster than disks.

---

# Cache Flow

```mermaid
graph LR

DISK["Disk"]

DISK --> CACHE["Page Cache"]

CACHE --> PROCESS["Application"]
```

---

# Memory Investigation

```bash
cat /proc/meminfo
```

---

# Filesystem Internals

Linux stores files using:

```text
Inodes
Data Blocks
```

---

# File Architecture

```mermaid
graph TD

NAME["Filename"]

NAME --> INODE["Inode"]

INODE --> BLOCKS["Data Blocks"]
```

---

# Inode Stores

```text
Owner

Permissions

Timestamps

Pointers
```

Not:

```text
Filename
```

---

# File Read Flow

```mermaid
flowchart TD

APP[Application]

APP --> OPEN[open]

OPEN --> INODE[Inode Lookup]

INODE --> BLOCKS[Read Blocks]

BLOCKS --> CACHE[Page Cache]

CACHE --> APP
```

---

# Linux VFS

Virtual Filesystem Layer.

Provides:

```text
Unified Interface
```

for:

```text
ext4
xfs
btrfs
zfs
```

---

# VFS Architecture

```mermaid
graph TD

APP["Application"]

APP --> VFS["VFS"]

VFS --> EXT4["ext4"]

VFS --> XFS["xfs"]

VFS --> BTRFS["btrfs"]
```

---

# Storage Stack

```mermaid
graph TD

APP["Application"]

APP --> FS["Filesystem"]

FS --> BLOCK["Block Layer"]

BLOCK --> DRIVER["Driver"]

DRIVER --> DISK["Disk"]
```

---

# Networking Internals

Applications use sockets.

---

# Networking Flow

```mermaid
graph LR

APP["Application"]

APP --> SOCKET["Socket"]

SOCKET --> TCP["TCP"]

TCP --> IP["IP"]

IP --> NIC["Network Card"]

NIC --> NETWORK["Network"]
```

---

# Socket Creation

```bash
socket()
bind()
listen()
accept()
```

Server flow.

---

# TCP Connection Flow

```mermaid
sequenceDiagram

Client->>Server: SYN

Server->>Client: SYN-ACK

Client->>Server: ACK
```

---

# Three-Way Handshake

Creates:

```text
Reliable Connection
```

---

# Network Commands

```bash
ss -tulpn

ip a

ip route

tcpdump
```

---

# Device Management

Linux treats devices as files.

Examples:

```text
/dev/sda

/dev/null

/dev/random

/dev/tty
```

---

# Device Architecture

```mermaid
graph TD

DEVICE["Hardware"]

DEVICE --> DRIVER["Driver"]

DRIVER --> DEV["/dev"]

DEV --> APP["Application"]
```

---

# Kernel Modules

Linux supports dynamic loading.

View:

```bash
lsmod
```

Load:

```bash
modprobe module
```

---

# Module Flow

```mermaid
graph LR

MODULE["Kernel Module"]

MODULE --> KERNEL["Kernel"]

KERNEL --> HARDWARE["Hardware"]
```

---

# Interrupts

Hardware signals CPU.

Examples:

```text
Network Packet

Disk Completion

Keyboard Input
```

---

# Interrupt Flow

```mermaid
graph LR

DEVICE["Device"]

DEVICE --> INTERRUPT["Interrupt"]

INTERRUPT --> CPU["CPU"]

CPU --> KERNEL["Kernel Handler"]
```

---

# Context Switching

CPU switches between processes.

---

# Context Switch Flow

```mermaid
graph TD

PROC1["Process A"]

PROC1 --> SAVE["Save State"]

SAVE --> LOAD["Load State"]

LOAD --> PROC2["Process B"]
```

---

# Namespaces

Foundation of containers.

Provide isolation.

Types:

```text
PID

Network

Mount

UTS

IPC

User
```

---

# Namespace Architecture

```mermaid
graph TD

HOST["Host"]

HOST --> NS1["Namespace 1"]

HOST --> NS2["Namespace 2"]

NS1 --> CONTAINER1["Container"]

NS2 --> CONTAINER2["Container"]
```

---

# cgroups

Control resources.

Limit:

```text
CPU

Memory

Disk

Network
```

---

# cgroup Architecture

```mermaid
graph TD

CGROUP["cgroup"]

CGROUP --> CPU["CPU Limit"]

CGROUP --> MEM["Memory Limit"]

CGROUP --> IO["IO Limit"]
```

---

# Containers Internals

A container is:

```text
Namespaces
+
cgroups
+
Filesystem Layers
```

Not:

```text
A VM
```

---

# Container Stack

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> NS["Namespaces"]

CONTAINER --> CG["cgroups"]

CONTAINER --> FS["OverlayFS"]
```

---

# OverlayFS

Docker storage technology.

---

# OverlayFS Layers

```mermaid
graph TD

BASE["Base Layer"]

BASE --> LAYER1["Layer"]

LAYER1 --> LAYER2["Layer"]

LAYER2 --> RW["Writable Layer"]
```

---

# /proc Filesystem

Kernel exposes process information.

Examples:

```bash
/proc/cpuinfo

/proc/meminfo

/proc/PID/status
```

---

# /sys Filesystem

Kernel device information.

Examples:

```bash
/sys/class

/sys/block

/sys/devices
```

---

# Linux Security Layers

```mermaid
graph TD

USER["User"]

USER --> PERM["Permissions"]

PERM --> ACL["ACL"]

ACL --> CAP["Capabilities"]

CAP --> SELINUX["SELinux/AppArmor"]
```

---

# Observability Layer

Linux provides visibility through:

```text
Logs

Metrics

Tracing

Events
```

---

# Key Investigation Commands

```bash
top

htop

ps

ss

strace

lsof

vmstat

iostat

dmesg

journalctl
```

---

# Universal Linux Internals Map

```mermaid
mindmap
  root((Linux))

    Kernel
      Scheduler
      Memory
      Drivers
      Network

    Processes
      PID
      Threads
      Signals

    Memory
      RAM
      Cache
      Virtual Memory

    Storage
      VFS
      Inodes
      Filesystems

    Networking
      TCP
      UDP
      Sockets

    Containers
      Namespaces
      cgroups
      OverlayFS

    Security
      Permissions
      ACLs
      Capabilities

    Observability
      Logs
      Metrics
      Tracing
```

---

# Production Debugging Cheat Sheet

CPU:

```bash
top
htop
pidstat
```

Memory:

```bash
free -h
vmstat
```

Storage:

```bash
iostat
df -h
```

Processes:

```bash
ps aux
pstree
```

Networking:

```bash
ss -tulpn
tcpdump
```

System Calls:

```bash
strace
```

Open Files:

```bash
lsof
```

Kernel:

```bash
dmesg
journalctl -k
```

---

# The Linux Engineer's Mental Model

When a user opens:

```text
https://example.com
```

Linux performs:

```text
DNS Resolution
        ↓
Socket Creation
        ↓
TCP Handshake
        ↓
Process Scheduling
        ↓
Memory Allocation
        ↓
Filesystem Access
        ↓
Network Transmission
        ↓
Response Delivery
```

Every Linux subsystem participates.

---

# Final Takeaway

Linux is not a collection of commands.

Linux is a living system made of:

```text
Processes
Memory
Storage
Networking
Security
Kernel
Containers
Observability
```

Commands merely expose the state of these subsystems.

Master Linux Internals and you gain the ability to:

```text
Debug Faster

Design Better Systems

Understand Containers

Operate Kubernetes

Scale Infrastructure

Think Like a Systems Engineer
```

The kernel is the heart of Linux.

Everything else is built around it.
