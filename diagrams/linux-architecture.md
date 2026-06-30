# Linux Architecture

## The Master Diagram of the Linux Operating System

---

# Why This Exists

Most Linux learners study topics separately:

```text
Filesystem
Processes
Memory
Networking
Storage
Security
systemd
Containers
```

The problem is:

Linux does not work as separate topics.

Linux is a collection of interconnected systems.

Real engineers understand:

> How everything connects.

This file serves as the master architecture map for the entire Linux Engineering Handbook.

If someone understands every diagram in this file, they will have a strong mental model of how Linux actually works.

---

# The Linux Mental Model

Linux is fundamentally a resource management system.

Its primary responsibility is managing:

```text
CPU
Memory
Storage
Network
Devices
Processes
```

Everything else is built on top.

---

# Linux From 10,000 Feet

```mermaid
graph TD

USER["Users"]

USER --> APPS["Applications"]

APPS --> LIBS["System Libraries"]

LIBS --> SYSCALL["System Calls"]

SYSCALL --> KERNEL["Linux Kernel"]

KERNEL --> CPU["CPU"]
KERNEL --> RAM["Memory"]
KERNEL --> DISK["Storage"]
KERNEL --> NET["Network"]
KERNEL --> DEV["Devices"]
```

---

# Complete Linux Stack

```text
+--------------------------------------------------+
|                     Users                        |
+--------------------------------------------------+

+--------------------------------------------------+
| Applications (Nginx, PostgreSQL, Docker, Bash)  |
+--------------------------------------------------+

+--------------------------------------------------+
| System Libraries (glibc, OpenSSL, libc++)       |
+--------------------------------------------------+

+--------------------------------------------------+
| System Call Interface                            |
+--------------------------------------------------+

+--------------------------------------------------+
| Linux Kernel                                     |
+--------------------------------------------------+

| Scheduler | Memory | Network | VFS | Drivers |

+--------------------------------------------------+
| Hardware                                         |
+--------------------------------------------------+
```

---

# Linux Architecture Overview

```mermaid
graph TD

LINUX["Linux Operating System"]

LINUX --> PROCESS["Process Management"]

LINUX --> MEMORY["Memory Management"]

LINUX --> STORAGE["Storage"]

LINUX --> NETWORK["Networking"]

LINUX --> SECURITY["Security"]

LINUX --> SYSTEMD["systemd"]

LINUX --> CONTAINERS["Containers"]

LINUX --> OBS["Observability"]
```

---

# Linux Boot Architecture

```mermaid
flowchart TD

POWER["Power On"]

POWER --> BIOS["BIOS / UEFI"]

BIOS --> BOOT["Bootloader (GRUB)"]

BOOT --> KERNEL["Linux Kernel"]

KERNEL --> INITRAMFS["Initramfs"]

INITRAMFS --> SYSTEMD["systemd PID 1"]

SYSTEMD --> SERVICES["Services"]

SERVICES --> USERSPACE["User Applications"]
```

---

# Linux Startup Sequence

```text
Power On
   ↓
Firmware
   ↓
Bootloader
   ↓
Kernel
   ↓
Initramfs
   ↓
systemd
   ↓
Services
   ↓
Applications
```

---

# User Space vs Kernel Space

One of the most important Linux concepts.

```mermaid
graph TD

USERSPACE["User Space"]

USERSPACE --> SYSCALL["System Calls"]

SYSCALL --> KERNELSPACE["Kernel Space"]

KERNELSPACE --> HARDWARE["Hardware"]
```

---

# User Space

Examples:

```text
Nginx
Docker
PostgreSQL
Python
Java
Node.js
Bash
```

Cannot directly access hardware.

---

# Kernel Space

Responsible for:

```text
CPU Scheduling
Memory Management
Networking
Filesystem Access
Device Control
Security
```

---

# Linux Kernel Architecture

```mermaid
graph TD

KERNEL["Linux Kernel"]

KERNEL --> SCHED["Scheduler"]

KERNEL --> MM["Memory Manager"]

KERNEL --> VFS["Virtual Filesystem"]

KERNEL --> NET["Network Stack"]

KERNEL --> DRIVER["Device Drivers"]

KERNEL --> SECURITY["Security Layer"]
```

---

# Process Architecture

Everything running on Linux is a process.

```mermaid
graph TD

PROCESS["Process"]

PROCESS --> THREADS["Threads"]

PROCESS --> MEMORY["Memory"]

PROCESS --> FILES["Open Files"]

PROCESS --> SOCKETS["Sockets"]

PROCESS --> SIGNALS["Signals"]
```

---

# Process Lifecycle

```mermaid
stateDiagram-v2

NEW --> READY

READY --> RUNNING

RUNNING --> WAITING

WAITING --> READY

RUNNING --> TERMINATED
```

---

# Process Hierarchy

```text
systemd (PID 1)
|
├── sshd
│    └── bash
│         └── vim
│
├── nginx
│
├── postgres
│
└── docker
```

---

# Process Creation Flow

```mermaid
graph LR

PARENT["Parent Process"]

PARENT --> FORK["fork()"]

FORK --> CHILD["Child Process"]

CHILD --> EXEC["exec()"]

EXEC --> PROGRAM["New Program"]
```

---

# Linux Scheduling Architecture

```mermaid
graph TD

READY["Ready Queue"]

READY --> SCHED["CFS Scheduler"]

SCHED --> CPU["CPU"]

CPU --> READY
```

---

# CPU Scheduling Model

```text
Processes
     ↓
Ready Queue
     ↓
Scheduler
     ↓
CPU
     ↓
Context Switch
     ↓
Next Process
```

---

# Memory Architecture

```mermaid
graph TD

PROCESS["Process"]

PROCESS --> VIRTUAL["Virtual Memory"]

VIRTUAL --> MMU["Memory Mapping"]

MMU --> PHYSICAL["Physical RAM"]
```

---

# Memory Layout

```text
+--------------------+
| Stack              |
+--------------------+
| Heap               |
+--------------------+
| Data Segment       |
+--------------------+
| Code Segment       |
+--------------------+
```

---

# Memory Management Architecture

```mermaid
graph LR

APP["Application"]

APP --> VIRTUAL["Virtual Memory"]

VIRTUAL --> PAGE["Page Tables"]

PAGE --> RAM["Physical Memory"]

RAM --> SWAP["Swap"]
```

---

# Page Cache Architecture

Linux aggressively caches data.

```mermaid
graph LR

DISK["Disk"]

DISK --> CACHE["Page Cache"]

CACHE --> APP["Application"]
```

---

# Filesystem Architecture

```mermaid
graph TD

APP["Application"]

APP --> VFS["Virtual Filesystem"]

VFS --> EXT4["ext4"]

VFS --> XFS["XFS"]

VFS --> BTRFS["Btrfs"]

EXT4 --> BLOCK["Block Layer"]

XFS --> BLOCK

BTRFS --> BLOCK
```

---

# Filesystem Internals

```mermaid
graph TD

FILENAME["Filename"]

FILENAME --> INODE["Inode"]

INODE --> BLOCKS["Data Blocks"]
```

---

# File Read Path

```mermaid
flowchart TD

APP["Application"]

APP --> OPEN["open()"]

OPEN --> INODE["Inode Lookup"]

INODE --> CACHE["Page Cache"]

CACHE --> BLOCKS["Disk Blocks"]

BLOCKS --> APP
```

---

# Storage Architecture

```mermaid
graph TD

APP["Application"]

APP --> FS["Filesystem"]

FS --> BLOCK["Block Layer"]

BLOCK --> DRIVER["Storage Driver"]

DRIVER --> DISK["Disk / SSD / NVMe"]
```

---

# Storage Stack

```text
Application
     ↓
Filesystem
     ↓
Block Layer
     ↓
Driver
     ↓
Storage Device
```

---

# Networking Architecture

```mermaid
graph LR

APP["Application"]

APP --> SOCKET["Socket"]

SOCKET --> TCP["TCP/UDP"]

TCP --> IP["IP Layer"]

IP --> NIC["Network Card"]

NIC --> NETWORK["Network"]
```

---

# Network Packet Flow

```mermaid
flowchart LR

APP["Application"]

APP --> SOCKET

SOCKET --> TCP

TCP --> IP

IP --> ROUTE["Routing"]

ROUTE --> NIC

NIC --> INTERNET["Network"]
```

---

# TCP Connection Architecture

```mermaid
sequenceDiagram

Client->>Server: SYN

Server->>Client: SYN-ACK

Client->>Server: ACK
```

---

# DNS Resolution Flow

```mermaid
sequenceDiagram

participant App
participant Resolver
participant DNS
participant Server

App->>Resolver: Domain

Resolver->>DNS: Lookup

DNS-->>Resolver: IP

Resolver-->>App: Result

App->>Server: Connect
```

---

# Device Management Architecture

Linux treats devices as files.

```mermaid
graph TD

DEVICE["Hardware"]

DEVICE --> DRIVER["Driver"]

DRIVER --> DEV["/dev"]

DEV --> APP["Applications"]
```

---

# Interrupt Architecture

```mermaid
graph LR

DEVICE["Device"]

DEVICE --> IRQ["Interrupt"]

IRQ --> CPU["CPU"]

CPU --> KERNEL["Kernel Handler"]
```

---

# Linux Security Architecture

```mermaid
graph TD

USER["User"]

USER --> PERM["Permissions"]

PERM --> ACL["ACLs"]

ACL --> CAP["Capabilities"]

CAP --> SELINUX["SELinux/AppArmor"]
```

---

# Authentication Architecture

```mermaid
graph TD

LOGIN["Login"]

LOGIN --> PAM["PAM"]

PAM --> PASSWD["/etc/passwd"]

PAM --> SHADOW["/etc/shadow"]

PAM --> ACCESS["Access Decision"]
```

---

# systemd Architecture

```mermaid
graph TD

SYSTEMD["PID 1"]

SYSTEMD --> SERVICES["Services"]

SYSTEMD --> TIMERS["Timers"]

SYSTEMD --> SOCKETS["Sockets"]

SYSTEMD --> MOUNTS["Mounts"]

SYSTEMD --> LOGS["journald"]
```

---

# Linux Logging Architecture

```mermaid
graph LR

APPLICATION["Application"]

APPLICATION --> JOURNALD["journald"]

JOURNALD --> STORAGE["Log Storage"]

STORAGE --> JOURNALCTL["journalctl"]
```

---

# Linux Observability Architecture

```mermaid
graph TD

SYSTEM["Linux"]

SYSTEM --> LOGS["Logs"]

SYSTEM --> METRICS["Metrics"]

SYSTEM --> EVENTS["Events"]

SYSTEM --> TRACES["Tracing"]
```

---

# Container Architecture

Containers are Linux kernel features.

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> NAMESPACE["Namespaces"]

CONTAINER --> CGROUP["cgroups"]

CONTAINER --> OVERLAY["OverlayFS"]
```

---

# Namespace Architecture

```mermaid
graph TD

HOST["Host"]

HOST --> PIDNS["PID Namespace"]

HOST --> NETNS["Network Namespace"]

HOST --> MOUNTNS["Mount Namespace"]

PIDNS --> CONTAINER["Container"]
```

---

# cgroup Architecture

```mermaid
graph TD

CGROUP["cgroup"]

CGROUP --> CPU["CPU Limit"]

CGROUP --> MEM["Memory Limit"]

CGROUP --> IO["I/O Limit"]
```

---

# Docker Architecture

```mermaid
graph TD

DOCKERCLI["Docker CLI"]

DOCKERCLI --> DOCKERD["dockerd"]

DOCKERD --> CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> CONTAINER["Container"]
```

---

# Kubernetes Foundation Architecture

```mermaid
graph TD

USER["User"]

USER --> API["API Server"]

API --> SCHED["Scheduler"]

API --> CONTROLLER["Controllers"]

SCHED --> NODE["Worker Node"]

NODE --> KUBELET["kubelet"]

KUBELET --> CONTAINER["Containers"]
```

---

# Cloud Infrastructure Architecture

```mermaid
graph TD

USER["Users"]

USER --> LB["Load Balancer"]

LB --> APP["Applications"]

APP --> CACHE["Cache"]

APP --> DB["Database"]

DB --> STORAGE["Persistent Storage"]
```

---

# Production Request Flow

A modern request typically follows:

```mermaid
flowchart LR

CLIENT["Client"]

CLIENT --> DNS

DNS --> LOADBALANCER["Load Balancer"]

LOADBALANCER --> APP["Application"]

APP --> CACHE["Redis"]

APP --> DB["Database"]

DB --> STORAGE["Storage"]
```

---

# Linux Troubleshooting Map

```mermaid
mindmap
  root((Troubleshooting))

    CPU
      top
      htop
      pidstat

    Memory
      free
      vmstat
      pmap

    Storage
      df
      du
      iostat

    Network
      ss
      tcpdump
      ip

    Services
      systemctl
      journalctl

    Containers
      docker
      kubectl
```

---

# The Universal Linux Data Flow

When a user requests a webpage:

```mermaid
flowchart TD

USER["User"]

USER --> DNS["DNS Lookup"]

DNS --> TCP["TCP Connection"]

TCP --> NGINX["Nginx"]

NGINX --> APP["Application"]

APP --> DB["Database"]

DB --> STORAGE["Disk"]

STORAGE --> DB

DB --> APP

APP --> NGINX

NGINX --> USER
```

---

# Linux Engineering Mind Map

```mermaid
mindmap
  root((Linux Engineering))

    Kernel
      Scheduling
      Memory
      Drivers

    Processes
      Threads
      Signals

    Filesystem
      Inodes
      VFS

    Storage
      RAID
      LVM

    Networking
      TCP
      DNS
      Routing

    Security
      Permissions
      Capabilities

    Containers
      Namespaces
      cgroups

    Kubernetes
      Pods
      Services

    Cloud
      Compute
      Storage
      Networking
```

---

# Final Takeaway

Linux is not a collection of commands.

Linux is a collection of interconnected subsystems:

```text
Processes
Memory
Storage
Filesystems
Networking
Security
systemd
Containers
Observability
```

Every production issue, cloud platform, container runtime, database, and distributed system eventually relies on these foundations.

Master the architecture, and every Linux topic becomes easier to understand because you can see where it fits in the larger system.
