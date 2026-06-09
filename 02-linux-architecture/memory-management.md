# Memory Management

Memory Management is one of the most critical responsibilities of the Linux kernel. It is responsible for allocating, tracking, protecting, optimizing, and reclaiming memory so that multiple applications can run efficiently and safely on the same system.

Linux uses advanced memory management techniques such as Virtual Memory, Paging, Page Tables, Demand Paging, Copy-on-Write (COW), NUMA, Slab Allocation, and Page Caching.

---

# Table of Contents

1. What is Memory Management?
2. Why Memory Management Exists
3. Memory Management Architecture
4. Physical Memory
5. Virtual Memory
6. Address Spaces
7. Process Memory Layout
8. Memory Allocation
9. Paging
10. Page Tables
11. MMU
12. TLB
13. Page Faults
14. Demand Paging
15. Swapping
16. Copy-on-Write (COW)
17. Memory Zones
18. NUMA
19. Slab Allocator
20. Page Cache
21. OOM Killer
22. Memory Monitoring Tools
23. Real-World Examples
24. Interview Questions

---

# What is Memory Management?

Memory Management is the subsystem that controls:

* RAM allocation
* Virtual memory
* Process memory isolation
* Swapping
* Memory protection
* Memory optimization

---

# Why Memory Management Exists

Without memory management:

```text
Application A
      │
      ▼
Access Memory of
Application B
```

Problems:

* Data corruption
* Security issues
* System crashes
* Resource starvation

Linux prevents this.

---

# Memory Management Architecture

```text
+--------------------------------+
|      User Applications         |
+--------------------------------+
              │
              ▼
+--------------------------------+
|      Virtual Memory Layer      |
+--------------------------------+
              │
              ▼
+--------------------------------+
|       Memory Manager           |
+--------------------------------+
              │
              ▼
+--------------------------------+
|        Physical Memory         |
|             RAM                |
+--------------------------------+
```

---

# Physical Memory

Physical memory refers to actual RAM installed in the machine.

Example:

```text
RAM
│
├── 4 GB
├── 8 GB
├── 16 GB
├── 32 GB
└── 64 GB
```

Physical RAM is limited.

---

# Virtual Memory

Virtual memory gives every process the illusion of having its own memory.

```text
Process A
      │
      ▼
Virtual Address Space

Process B
      │
      ▼
Virtual Address Space

Kernel
      │
      ▼
Physical RAM
```

Benefits:

* Isolation
* Security
* Flexibility
* Larger address space

---

# Virtual Memory Mapping

```text
Virtual Address
       │
       ▼
Page Table
       │
       ▼
Physical Address
```

Example:

```text
Virtual Address:
0x1000

Mapped To

Physical Address:
0xA500
```

---

# Process Address Space

Each process receives its own address space.

```text
+----------------------+
| Kernel Space         |
+----------------------+
| Stack                |
+----------------------+
| Memory Mapped Files  |
+----------------------+
| Heap                 |
+----------------------+
| Data Segment         |
+----------------------+
| Text Segment         |
+----------------------+
```

---

# Text Segment

Contains executable instructions.

```text
Program Code
```

Example:

```c
int main() {
}
```

Stored in Text Segment.

---

# Data Segment

Stores initialized global variables.

Example:

```c
int count = 10;
```

---

# BSS Segment

Stores uninitialized variables.

Example:

```c
int count;
```

---

# Heap

Dynamic memory allocation area.

Functions:

```c
malloc()
calloc()
realloc()
free()
```

Visualization:

```text
Heap Growth
     ▲
     │
     │
```

---

# Stack

Stores:

* Function calls
* Local variables
* Return addresses

Visualization:

```text
Stack Growth
     ▼
```

---

# Complete Process Memory Layout

```text
High Memory
+---------------------+
| Kernel Space        |
+---------------------+
| Stack               |
+---------------------+
| Shared Libraries    |
+---------------------+
| Memory Mapped Files |
+---------------------+
| Heap                |
+---------------------+
| BSS                 |
+---------------------+
| Data Segment        |
+---------------------+
| Text Segment        |
+---------------------+
Low Memory
```

---

# Memory Allocation Flow

```text
Application
      │
      ▼
malloc()
      │
      ▼
glibc
      │
      ▼
brk() / mmap()
      │
      ▼
Kernel
      │
      ▼
RAM
```

---

# Paging

Linux divides memory into fixed-size blocks called Pages.

Typical Page Size:

```text
4 KB
```

---

# Paging Architecture

```text
Virtual Memory

+-------+
| Page1 |
+-------+
| Page2 |
+-------+
| Page3 |
+-------+

       │
       ▼

Physical Memory

+-------+
|Frame5 |
+-------+
|Frame1 |
+-------+
|Frame9 |
+-------+
```

Pages do not need contiguous memory.

---

# Pages vs Frames

| Term  | Meaning               |
| ----- | --------------------- |
| Page  | Virtual Memory Block  |
| Frame | Physical Memory Block |

Example:

```text
Page 1 → Frame 4
Page 2 → Frame 9
Page 3 → Frame 1
```

---

# Page Tables

Page tables store memory mappings.

```text
Virtual Page
      │
      ▼
Page Table
      │
      ▼
Physical Frame
```

Example:

```text
Page 1 → Frame 7
Page 2 → Frame 2
Page 3 → Frame 9
```

---

# Memory Management Unit (MMU)

MMU translates addresses.

```text
CPU
 │
 ▼
Virtual Address
 │
 ▼
MMU
 │
 ▼
Physical Address
 │
 ▼
RAM
```

Without MMU, virtual memory cannot exist.

---

# Translation Lookaside Buffer (TLB)

TLB is a cache for page table entries.

```text
CPU
 │
 ▼
TLB Lookup
 │
 ├── Hit
 │     │
 │     ▼
 │   RAM
 │
 └── Miss
       │
       ▼
   Page Table
```

Benefits:

* Faster memory access
* Reduced page table lookups

---

# Page Fault

Occurs when a page is not in memory.

```text
Process
   │
   ▼
Access Page
   │
   ▼
Page Missing
   │
   ▼
Page Fault
   │
   ▼
Kernel Loads Page
```

---

# Demand Paging

Pages are loaded only when needed.

Without Demand Paging:

```text
Entire Program Loaded
```

With Demand Paging:

```text
Load Page Only When Used
```

Benefits:

* Faster startup
* Reduced RAM usage

---

# Swapping

When RAM is full:

```text
RAM
 │
 ▼
Swap Space
```

Swap can be:

```text
Swap Partition
Swap File
```

---

# Swapping Flow

```text
RAM Full
    │
    ▼
Select Unused Pages
    │
    ▼
Write To Disk
    │
    ▼
Free RAM
```

---

# Copy-on-Write (COW)

Used during process creation.

Example:

```c
fork()
```

Initially:

```text
Parent
   │
   ▼
Shared Pages
   ▲
   │
Child
```

When modified:

```text
Parent Page
     │
     ▼
Copy Created
     │
     ▼
Child Page
```

Benefits:

* Faster fork()
* Reduced memory usage

---

# Memory Zones

Linux divides memory into zones.

```text
Memory
│
├── DMA
├── DMA32
├── Normal
└── High Memory
```

---

# DMA Zone

Used by devices requiring direct memory access.

```text
Device
   │
   ▼
DMA Memory
```

---

# NUMA (Non-Uniform Memory Access)

Used in multi-CPU systems.

```text
CPU 1 ── Local RAM

CPU 2 ── Local RAM
```

Accessing local memory is faster.

---

# NUMA Architecture

```text
      CPU 1
        │
        ▼
     RAM 1

        │

        ▼

     RAM 2
        ▲
        │
      CPU 2
```

---

# Slab Allocator

Kernel frequently allocates small objects.

Examples:

```text
Process Structures
File Objects
Network Buffers
```

---

# Slab Architecture

```text
Cache
 │
 ├── Slab
 │     ├── Object
 │     ├── Object
 │     └── Object
 │
 └── Slab
```

Benefits:

* Faster allocation
* Reduced fragmentation

---

# Page Cache

Caches file data in memory.

```text
Application
      │
      ▼
Page Cache
      │
      ▼
Disk
```

Benefits:

* Faster file access
* Reduced disk I/O

---

# Page Cache Example

First Read:

```text
Application
      │
      ▼
Disk
```

Second Read:

```text
Application
      │
      ▼
Page Cache
```

Much faster.

---

# Out Of Memory (OOM) Killer

When memory is exhausted:

```text
RAM Full
     │
     ▼
OOM Killer Activated
     │
     ▼
Select Process
     │
     ▼
Terminate Process
```

Purpose:

Prevent total system crash.

---

# Memory Monitoring Tools

## Free Memory

```bash
free -h
```

---

## Virtual Memory Stats

```bash
vmstat
```

---

## Memory Usage

```bash
top
```

---

## Detailed Memory

```bash
cat /proc/meminfo
```

---

# Real-World Example

Opening Chrome

```text
Chrome
   │
   ▼
Virtual Memory Created
   │
   ▼
Pages Allocated
   │
   ▼
Libraries Mapped
   │
   ▼
RAM Assigned
```

---

# Real-World Example: fork()

```text
Parent Process
       │
       ▼
fork()
       │
       ▼
Copy-On-Write Pages
       │
       ▼
Child Process
```

---

# Memory Optimization Techniques

```text
Memory Optimization
│
├── Paging
├── Demand Paging
├── TLB
├── Page Cache
├── NUMA
├── Slab Allocation
└── Copy-On-Write
```

---

# Advantages

### Isolation

Processes cannot access each other's memory.

### Security

Memory protection mechanisms.

### Performance

Caching and TLB improve speed.

### Scalability

Supports large workloads.

### Reliability

OOM protection and paging.

---

# Disadvantages

### Complexity

Memory subsystem is large.

### Swapping Overhead

Disk is slower than RAM.

### Page Fault Cost

Requires kernel intervention.

### Fragmentation

Can occur over time.

---

# Production Systems Using Linux Memory Management

```text
Cloud Servers
Docker Hosts
Kubernetes Clusters
Android Devices
Databases
Web Servers
AI Training Systems
Supercomputers
Virtual Machines
```

---

# Interview Questions

## Beginner

1. What is virtual memory?
2. Difference between RAM and virtual memory?
3. What is a page?
4. What is a page fault?
5. What is swapping?

## Intermediate

6. Explain MMU.
7. What is TLB?
8. Explain page tables.
9. What is demand paging?
10. Explain Copy-on-Write.

## Advanced

11. How does Linux implement virtual memory?
12. Explain NUMA architecture.
13. How does the slab allocator work?
14. What is page cache?
15. How does the OOM killer choose a process?

---

# Summary

Linux Memory Management provides secure, isolated, and efficient memory usage through Virtual Memory, Paging, MMU translation, Page Tables, TLB caching, Demand Paging, Copy-on-Write, NUMA optimization, Slab Allocation, and Page Caching. These mechanisms allow Linux to efficiently run everything from embedded systems and smartphones to cloud platforms and supercomputers while maintaining performance, security, and stability.
