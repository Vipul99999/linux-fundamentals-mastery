# Proc Filesystem (procfs) in Linux

> The Proc Filesystem (`procfs`) is one of the most important virtual filesystems in Linux.
>
> It provides a window into the Linux kernel, processes, memory, CPU, networking, and system state.
>
> Without `/proc`, tools like:
>
> ```bash
> ps
> top
> htop
> free
> uptime
> vmstat
> netstat
> ```
>
> would not function the way they do today.

---

# Table of Contents

1. What is ProcFS?
2. Why ProcFS Exists
3. ProcFS Architecture
4. How ProcFS Works
5. Mounting ProcFS
6. ProcFS Directory Structure
7. Process Directories
8. System Information Files
9. Memory Information
10. CPU Information
11. Kernel Information
12. Networking Information
13. Process Inspection
14. Process File Descriptors
15. Process Memory Maps
16. Kernel Parameters
17. ProcFS and VFS
18. Real World Usage
19. Security Considerations
20. Troubleshooting
21. Interview Questions

---

# What is ProcFS?

ProcFS (Process Filesystem) is a virtual filesystem that exposes kernel and process information as files.

Mount point:

```text
/proc
```

---

# Important

Files inside `/proc`:

```text
Do Not Exist On Disk
```

They are generated dynamically by the kernel.

---

# Visual

```text
cat /proc/cpuinfo

       │

       ▼

 Kernel Data

       │

       ▼

 Generated Output
```

---

# Why ProcFS Exists

Without ProcFS:

Applications would need:

```text
Special System Calls
Kernel APIs
Custom Drivers
```

to retrieve information.

---

Linux solution:

```text
Expose Kernel Data As Files
```

---

# Visual

```text
User Program
      │
      ▼
Read File
      │
      ▼
Kernel Generates Data
```

---

# ProcFS Architecture

```text
Application
      │
      ▼
cat /proc/meminfo
      │
      ▼
VFS
      │
      ▼
procfs Driver
      │
      ▼
Kernel Memory
      │
      ▼
Generated Output
```

---

# Linux Filesystem Integration

```text
Applications
      │
      ▼
System Calls
      │
      ▼
VFS
      │
      ├── ext4
      ├── xfs
      └── procfs
```

---

# Mounting ProcFS

Check:

```bash
mount | grep proc
```

---

Example:

```text
proc on /proc type proc
```

---

# Manual Mount

```bash
mount -t proc proc /proc
```

---

# ProcFS Directory Structure

```text
/proc
│
├── cpuinfo
├── meminfo
├── uptime
├── version
├── loadavg
├── modules
├── interrupts
├── filesystems
├── sys
└── PID Directories
```

---

# Visual Overview

```text
/proc
│
├── System Information
│
├── Kernel Information
│
├── Hardware Information
│
└── Process Information
```

---

# Process Directories

One directory exists for every running process.

---

Example:

```text
/proc/1
/proc/100
/proc/500
/proc/2000
```

---

Directory name equals:

```text
PID
```

(Process ID)

---

# Visual

```text
Running Processes
       │
       ▼
PID 1
PID 100
PID 500
PID 1000
       │
       ▼
/proc/PID
```

---

# Process Directory Structure

Example:

```text
/proc/1234
│
├── cmdline
├── cwd
├── environ
├── exe
├── fd
├── maps
├── stat
├── status
└── task
```

---

# cmdline

Shows process command line.

---

Example

```bash
cat /proc/1234/cmdline
```

Output:

```text
python app.py
```

---

# exe

Points to executable.

---

Example

```bash
ls -l /proc/1234/exe
```

Output:

```text
/usr/bin/python3
```

---

# cwd

Current Working Directory.

---

Example

```bash
ls -l /proc/1234/cwd
```

Output:

```text
/home/user/project
```

---

# fd (File Descriptors)

Shows all open files.

---

Visual

```text
/proc/1234/fd
│
├── 0
├── 1
├── 2
├── 3
└── 4
```

---

# Meaning

```text
0 → stdin
1 → stdout
2 → stderr
```

---

# Example

```bash
ls -l /proc/1234/fd
```

---

# Visual

```text
Process
     │
     ▼
Open Files
     │
     ▼
fd Directory
```

---

# status

Human-readable process information.

---

Example

```bash
cat /proc/1234/status
```

---

Contains:

```text
Name
PID
Memory Usage
Threads
State
UID
GID
```

---

# stat

Machine-readable process statistics.

---

Example

```bash
cat /proc/1234/stat
```

---

Contains:

```text
CPU Time
Priority
State
Memory
Scheduling Data
```

---

# task

Shows process threads.

---

Visual

```text
Process
    │
    ▼
Threads
    │
    ▼
/proc/PID/task
```

---

# CPU Information

File:

```text
/proc/cpuinfo
```

---

Example

```bash
cat /proc/cpuinfo
```

---

Contains:

```text
Processor
Model
Vendor
Cache
Flags
Frequency
```

---

# Visual

```text
CPU Hardware
      │
      ▼
Kernel
      │
      ▼
/proc/cpuinfo
```

---

# Memory Information

File:

```text
/proc/meminfo
```

---

Example

```bash
cat /proc/meminfo
```

---

Contains:

```text
Total RAM
Free RAM
Buffers
Cached Memory
Swap
```

---

# Visual

```text
Physical Memory
       │
       ▼
Kernel
       │
       ▼
/proc/meminfo
```

---

# Uptime Information

File:

```text
/proc/uptime
```

---

Example

```bash
cat /proc/uptime
```

Output:

```text
125000.22 450000.10
```

---

Meaning:

```text
System Uptime
CPU Idle Time
```

---

# Load Average

File:

```text
/proc/loadavg
```

---

Example

```bash
cat /proc/loadavg
```

Output:

```text
0.25 0.40 0.50
```

---

Represents:

```text
1 Minute Load
5 Minute Load
15 Minute Load
```

---

# Kernel Version

File:

```text
/proc/version
```

---

Example

```bash
cat /proc/version
```

---

Contains:

```text
Kernel Version
Compiler
Build Information
```

---

# Filesystem Information

File:

```text
/proc/filesystems
```

---

Example

```bash
cat /proc/filesystems
```

---

Output

```text
ext4
xfs
btrfs
proc
tmpfs
```

---

# Loaded Modules

File:

```text
/proc/modules
```

---

Example

```bash
cat /proc/modules
```

---

Shows:

```text
Loaded Kernel Modules
```

---

# Interrupt Information

File:

```text
/proc/interrupts
```

---

Example

```bash
cat /proc/interrupts
```

---

Displays:

```text
Hardware Interrupt Counts
```

---

# Network Information

Directory:

```text
/proc/net
```

---

Visual

```text
/proc/net
│
├── tcp
├── udp
├── route
├── dev
└── arp
```

---

# Example

```bash
cat /proc/net/tcp
```

---

Shows:

```text
TCP Connections
```

---

# Routing Table

```bash
cat /proc/net/route
```

---

Contains:

```text
Kernel Routing Table
```

---

# Process Memory Maps

File:

```text
/proc/PID/maps
```

---

Example

```bash
cat /proc/1234/maps
```

---

Shows:

```text
Executable Memory
Shared Libraries
Stack
Heap
Mapped Files
```

---

# Visual

```text
Process Memory
│
├── Code
├── Heap
├── Stack
└── Libraries
```

---

# Environment Variables

File:

```text
/proc/PID/environ
```

---

Example

```bash
cat /proc/1234/environ
```

---

Contains:

```text
PATH
HOME
USER
SHELL
```

---

# Kernel Parameters

Directory:

```text
/proc/sys
```

---

Visual

```text
/proc/sys
│
├── kernel
├── vm
├── net
└── fs
```

---

# Example

```bash
cat /proc/sys/kernel/hostname
```

---

Output:

```text
my-server
```

---

# Modify Kernel Parameters

Temporary change:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

---

Visual

```text
User
 │
 ▼
Write To ProcFS
 │
 ▼
Kernel Setting Updated
```

---

# ProcFS and VFS

Applications access ProcFS exactly like normal files.

---

Visual

```text
Application
      │
      ▼
open()
read()
close()
      │
      ▼
VFS
      │
      ▼
procfs
```

---

# Real World Usage

---

# ps Command

Reads:

```text
/proc
```

to display processes.

---

# top / htop

Read:

```text
/proc/stat
/proc/meminfo
```

---

# free Command

Reads:

```text
/proc/meminfo
```

---

# uptime Command

Reads:

```text
/proc/uptime
```

---

# Docker

Uses:

```text
/proc
```

for process visibility.

---

# Kubernetes

Uses:

```text
procfs
cgroups
```

extensively.

---

# Security Considerations

Some ProcFS files contain sensitive data.

---

Examples:

```text
/proc/PID/environ
/proc/PID/maps
/proc/PID/fd
```

---

Access is controlled by:

```text
Permissions
Capabilities
Namespaces
```

---

# Common Misconceptions

❌ /proc files are stored on disk

✔ Generated dynamically

---

❌ Editing proc files edits text files

✔ Often changes kernel state

---

❌ Process directories are permanent

✔ Exist only while process runs

---

❌ /proc only contains process data

✔ Also contains kernel, memory, CPU, networking, and system information

---

# Troubleshooting Commands

---

## Show CPU Information

```bash
cat /proc/cpuinfo
```

---

## Show Memory Information

```bash
cat /proc/meminfo
```

---

## Show Uptime

```bash
cat /proc/uptime
```

---

## Show Running Processes

```bash
ls /proc | grep '^[0-9]'
```

---

## Show Open Files

```bash
ls -l /proc/PID/fd
```

---

## Show Network Information

```bash
cat /proc/net/dev
```

---

# Complete ProcFS Map

```text
/proc
│
├── cpuinfo
│
├── meminfo
│
├── uptime
│
├── version
│
├── modules
│
├── interrupts
│
├── filesystems
│
├── net
│
├── sys
│
└── PID Directories
    │
    ├── status
    ├── stat
    ├── maps
    ├── fd
    ├── environ
    ├── exe
    └── task
```

---

# Interview Questions

## Beginner

1. What is ProcFS?
2. Why does Linux use ProcFS?
3. Is ProcFS stored on disk?
4. What is `/proc/cpuinfo`?
5. What is `/proc/meminfo`?
6. What does `/proc/uptime` contain?
7. What is a PID directory?
8. What is `/proc/PID/status`?
9. What is `/proc/PID/fd`?
10. What is `/proc/sys`?

---

## Intermediate

11. Explain ProcFS architecture.
12. Explain process directories.
13. Explain file descriptor tracking.
14. Explain memory maps.
15. Explain environment variable storage.
16. Explain kernel parameter management.
17. Explain network information exposure.
18. Explain module information.
19. Explain VFS integration.
20. Explain namespace interactions.

---

## Advanced

21. Explain procfs implementation.
22. Explain seq_file API.
23. Explain process information generation.
24. Explain procfs security controls.
25. Explain PID namespaces and ProcFS.
26. Explain procfs caching.
27. Explain kernel parameter interfaces.
28. Explain performance implications.
29. Explain container procfs isolation.
30. Explain procfs internals in the Linux kernel.

---

# Summary

```text
Running Kernel
       │
       ▼
Kernel Data Structures
       │
       ▼
ProcFS
       │
       ▼
/proc Files
       │
       ▼
User Applications
```

Core Idea:

```text
ProcFS
      =
Kernel Information
      +
Process Information
      +
System Information
      +
Runtime State
```

ProcFS is the foundation for:

- Process Monitoring
- System Monitoring
- Performance Analysis
- Debugging
- Container Runtime Systems
- Kubernetes
- Docker
- Linux Administration
- Linux Kernel Observability
