# Linux Internals Mind Map
## Understanding What Happens Under the Hood

> Most Linux users learn commands.
>
> Engineers learn systems.
>
> Great engineers learn internals.
>
> This file explains Linux as an interconnected machine.
>
> Goal:
>
> - Understand how Linux actually works
> - Build deep engineering intuition
> - Learn kernel-level thinking
> - Connect Linux to Docker, Kubernetes, Databases, Cloud, and Distributed Systems
> - Become a better troubleshooter

---

# The Linux Internals Universe

```mermaid
mindmap
root((Linux Internals))

    Hardware
        CPU
        RAM
        Storage
        NIC
        Interrupts

    Boot Process
        BIOS
        UEFI
        GRUB
        Kernel
        Initramfs
        systemd

    Kernel
        Scheduler
        Memory Manager
        VFS
        Drivers
        Security
        Networking

    Processes
        PID
        Threads
        Signals
        Context Switching

    Memory
        Virtual Memory
        Paging
        Swap
        Cache
        Buffers

    Storage
        Block Devices
        Filesystems
        Inodes
        Journaling

    Networking
        Sockets
        TCP
        UDP
        Routing
        Netfilter

    Security
        Users
        Permissions
        Capabilities
        SELinux

    Containers
        Namespaces
        Cgroups
        OverlayFS

    Observability
        Logs
        Metrics
        Traces
        eBPF
```

---

# Linux Architecture Overview

```mermaid
flowchart TD

Users

--> Applications

Applications

--> Libraries

Libraries

--> SystemCalls

SystemCalls

--> Kernel

Kernel

--> Hardware

Hardware

--> CPU

Hardware

--> RAM

Hardware

--> Disk

Hardware

--> NIC
```

---

# Complete Linux Internal Stack

```text
+------------------------------------------------+
|                 User Applications              |
+------------------------------------------------+
|                Shared Libraries                |
+------------------------------------------------+
|                System Call Layer               |
+------------------------------------------------+
|                     Kernel                     |
|------------------------------------------------|
| Scheduler | Memory | Network | VFS | Security |
+------------------------------------------------+
|                Device Drivers                  |
+------------------------------------------------+
|          CPU | RAM | SSD | NIC | GPU           |
+------------------------------------------------+
```

---

# Linux Boot Process Deep Dive

```mermaid
flowchart TD

PowerOn

--> BIOS_UEFI

--> HardwareInitialization

--> GRUB

--> KernelLoad

--> Initramfs

--> RootFilesystem

--> systemd

--> Services

--> Login

--> UserSession
```

---

# Linux Startup Timeline

```text
Power Button Pressed
        │
        ▼
BIOS / UEFI
        │
        ▼
Hardware Detection
        │
        ▼
Bootloader (GRUB)
        │
        ▼
Kernel Loaded
        │
        ▼
Initramfs
        │
        ▼
Root Filesystem Mounted
        │
        ▼
PID 1 (systemd)
        │
        ▼
Services Start
        │
        ▼
Login Available
```

---

# Kernel Internal Architecture

```mermaid
flowchart TD

Kernel

--> Scheduler

Kernel

--> MemoryManager

Kernel

--> VFS

Kernel

--> NetworkStack

Kernel

--> Security

Kernel

--> DeviceDrivers

Scheduler --> CPU

MemoryManager --> RAM

VFS --> Filesystems

NetworkStack --> NIC

DeviceDrivers --> Hardware
```

---

# Kernel Subsystem Map

```text
KERNEL
│
├── Process Scheduler
│   ├── CFS
│   ├── Priorities
│   ├── CPU Affinity
│   └── Context Switching
│
├── Memory Manager
│   ├── Virtual Memory
│   ├── Paging
│   ├── Swap
│   ├── Slab Allocator
│   └── OOM Killer
│
├── VFS
│   ├── Inodes
│   ├── Dentries
│   ├── File Handles
│   └── Mounts
│
├── Network Stack
│   ├── TCP
│   ├── UDP
│   ├── Routing
│   ├── Firewall
│   └── Sockets
│
├── Security
│   ├── Permissions
│   ├── Capabilities
│   ├── SELinux
│   └── Audit
│
└── Device Drivers
    ├── Disk
    ├── Network
    ├── USB
    ├── GPU
    └── Input Devices
```

---

# Process Internals

```mermaid
mindmap
root((Processes))

    PID

    PPID

    Threads

    Scheduler

    ContextSwitch

    Signals

    ProcessStates

    Fork

    Exec

    Zombie

    Orphan
```

---

# Process Life Cycle

```mermaid
stateDiagram-v2

[*] --> New

New --> Ready

Ready --> Running

Running --> Waiting

Waiting --> Ready

Running --> Terminated

Terminated --> [*]
```

---

# What Happens When You Run a Program?

Example:

```bash
nginx
```

```mermaid
flowchart TD

User

--> Shell

Shell

--> fork()

fork()

--> ChildProcess

ChildProcess

--> exec()

exec()

--> ProgramLoaded

ProgramLoaded

--> Scheduler

Scheduler

--> CPUExecution
```

---

# Process Memory Layout

```text
High Memory
+--------------------+
| Kernel Space       |
+--------------------+

| Stack              |
+--------------------+

| Heap               |
+--------------------+

| BSS                |
+--------------------+

| Data Segment       |
+--------------------+

| Text Segment       |
+--------------------+

Low Memory
```

---

# Context Switching

```mermaid
flowchart LR

ProcessA

--> CPU

CPU

--> SaveState

SaveState

--> Scheduler

Scheduler

--> LoadState

LoadState

--> ProcessB
```

---

# Linux Memory Architecture

```mermaid
mindmap
root((Memory))

    PhysicalRAM

    VirtualMemory

    Paging

    Swap

    HugePages

    NUMA

    SlabAllocator

    PageCache

    Buffers

    OOMKiller
```

---

# Virtual Memory Flow

```mermaid
flowchart TD

Application

--> VirtualAddress

VirtualAddress

--> MMU

MMU

--> PageTable

PageTable

--> PhysicalRAM
```

---

# Memory Request Journey

```mermaid
flowchart TD

Application

--> malloc()

--> VirtualMemory

--> PageTable

--> RAM

RAM

--> CPU
```

---

# OOM Killer Decision Flow

```mermaid
flowchart TD

MemoryPressure

--> FreeMemory

FreeMemory --> EnoughRAM

FreeMemory --> NotEnoughRAM

NotEnoughRAM --> SwapUsage

SwapUsage --> OOMKiller

OOMKiller --> KillProcess
```

---

# Filesystem Internals

```mermaid
mindmap
root((Filesystem))

    VFS

    Inodes

    Dentries

    Superblock

    Journaling

    Mounts

    BlockDevices

    EXT4

    XFS

    BTRFS
```

---

# File Read Journey

```mermaid
flowchart LR

Application

--> read()

--> VFS

--> Filesystem

--> BlockLayer

--> Disk

--> PageCache

--> Application
```

---

# Inode Architecture

```text
File Name
    │
    ▼
Directory Entry
    │
    ▼
Inode
    │
    ├── Permissions
    ├── Owner
    ├── Size
    ├── Timestamps
    └── Block Pointers
              │
              ▼
        Data Blocks
```

---

# Linux Storage Stack

```mermaid
flowchart TD

Application

--> Filesystem

--> VFS

--> BlockLayer

--> DeviceDriver

--> SSD

--> NANDFlash
```

---

# Linux Networking Internals

```mermaid
mindmap
root((Networking))

    Sockets

    TCP

    UDP

    Routing

    ARP

    DNS

    Netfilter

    Firewall

    Interfaces

    Queues
```

---

# Network Packet Journey

```mermaid
flowchart LR

Application

--> Socket

--> TCP

--> IP

--> NICDriver

--> NIC

--> Switch

--> Router

--> Internet
```

---

# TCP Stack Internals

```mermaid
flowchart TD

Application

--> Socket

--> TCP

--> IP

--> Ethernet

--> NIC
```

---

# TCP Three-Way Handshake

```mermaid
sequenceDiagram

Client->>Server: SYN

Server->>Client: SYN ACK

Client->>Server: ACK
```

---

# Linux Interrupt Architecture

```mermaid
flowchart TD

HardwareEvent

--> Interrupt

--> CPU

--> InterruptHandler

--> Kernel

--> Driver

--> Process
```

---

# Device Driver Architecture

```mermaid
flowchart TD

Application

--> SystemCall

--> Kernel

--> Driver

--> Hardware
```

---

# Linux Security Internals

```mermaid
mindmap
root((Security))

    UID

    GID

    Permissions

    ACL

    Capabilities

    PAM

    SELinux

    AppArmor

    Audit
```

---

# Permission Check Flow

```mermaid
flowchart TD

User

--> FileAccess

FileAccess

--> UIDCheck

UIDCheck

--> PermissionCheck

PermissionCheck

--> Allow

PermissionCheck

--> Deny
```

---

# System Call Journey

Example:

```bash
cat file.txt
```

```mermaid
flowchart TD

User

--> Shell

Shell

--> read()

read()

--> SystemCall

SystemCall

--> Kernel

Kernel

--> VFS

VFS

--> Filesystem

Filesystem

--> Disk

Disk

--> DataReturned
```

---

# Namespace Architecture (Containers)

```mermaid
mindmap
root((Namespaces))

    PID

    Network

    Mount

    User

    IPC

    UTS

    Cgroup
```

---

# Cgroup Architecture

```mermaid
flowchart TD

Container

--> CPUQuota

Container

--> MemoryLimit

Container

--> DiskLimit

Container

--> NetworkLimit
```

---

# Docker Exists Because Of Linux

```mermaid
flowchart TD

LinuxKernel

--> Namespaces

LinuxKernel

--> Cgroups

LinuxKernel

--> OverlayFS

Namespaces

--> Isolation

Cgroups

--> ResourceControl

OverlayFS

--> LayeredFilesystems

Isolation

--> Containers
```

---

# eBPF Architecture

```mermaid
flowchart TD

Application

--> eBPFProgram

eBPFProgram

--> KernelHooks

KernelHooks

--> Network

KernelHooks

--> Processes

KernelHooks

--> Filesystem

KernelHooks

--> Security
```

---

# Linux Internals Dependency Graph

```mermaid
graph TD

CPU --> Scheduler

RAM --> MemoryManager

Disk --> Filesystem

NIC --> NetworkStack

Scheduler --> Processes

MemoryManager --> Applications

Filesystem --> Databases

NetworkStack --> WebServers

Namespaces --> Docker

Docker --> Kubernetes

Kubernetes --> Cloud

Cloud --> DistributedSystems
```

---

# The Ultimate Mental Model

```text
Hardware
   │
   ▼
Device Drivers
   │
   ▼
Kernel
   │
   ├── Scheduler
   ├── Memory Manager
   ├── VFS
   ├── Network Stack
   ├── Security
   │
   ▼
System Calls
   │
   ▼
Libraries
   │
   ▼
Applications
   │
   ▼
Containers
   │
   ▼
Kubernetes
   │
   ▼
Cloud
   │
   ▼
Internet Scale Systems
```

---

# Final Engineering Truth

```text
Every Linux problem eventually becomes one of:

CPU
Memory
Storage
Network
Kernel
Process
Filesystem
Security

Master these eight areas and you can troubleshoot
almost every production Linux issue.

Linux Internals is not about commands.

Linux Internals is about understanding how the machine thinks.
```

**End of Linux Internals Mind Map**
