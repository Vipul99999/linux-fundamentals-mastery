# Visual Index

## The Master Navigation Map for the Linux Engineering Handbook

> This file is the **visual table of contents** for the entire repository.
>
> Instead of navigating through folders, readers can use this file as a systems-thinking map.
>
> Every topic in this repository ultimately connects to Linux.

---

# How to Use This File

There are two ways to learn Linux:

### Path 1: Topic-Based Learning

```text
Linux
 ├── Filesystem
 ├── Processes
 ├── Networking
 ├── Storage
 ├── Security
 └── Bash
```

### Path 2: Systems Thinking

```text
Linux
   ↓
Containers
   ↓
Kubernetes
   ↓
Cloud
   ↓
Databases
   ↓
Distributed Systems
   ↓
Internet
```

This repository follows both approaches.

---

# Repository Knowledge Graph

```mermaid
graph TD

LINUX["Linux Fundamentals"]

LINUX --> ARCH["Linux Architecture"]

ARCH --> FS["Filesystem"]

ARCH --> PROCESS["Processes"]

ARCH --> MEMORY["Memory"]

ARCH --> NETWORK["Networking"]

ARCH --> STORAGE["Storage"]

ARCH --> SECURITY["Security"]

FS --> SYSTEMD["systemd"]

PROCESS --> SYSTEMD

MEMORY --> TROUBLE["Troubleshooting"]

NETWORK --> TROUBLE

STORAGE --> TROUBLE

SECURITY --> TROUBLE

SYSTEMD --> BASH["Bash"]

BASH --> DOCKER["Docker"]

DOCKER --> K8S["Kubernetes"]

K8S --> CLOUD["Cloud"]

CLOUD --> DATABASE["Databases"]

DATABASE --> OBS["Observability"]

OBS --> DIST["Distributed Systems"]

DIST --> INTERNET["Internet"]

INTERNET --> PLATFORM["Platform Engineering"]

PLATFORM --> SRE["SRE"]

SRE --> ARCHITECT["System Architecture"]
```

---

# Learning Journey Map

```mermaid
journey
    title Linux Engineering Growth Path

    section Foundations
      Linux Basics: 5
      Filesystem: 5
      Processes: 5
      Permissions: 5

    section Intermediate
      Networking: 4
      Storage: 4
      systemd: 4
      Bash: 4

    section Advanced
      Performance: 3
      Security: 3
      Troubleshooting: 3

    section Modern Infrastructure
      Docker: 3
      Kubernetes: 2
      Cloud: 2

    section Senior Engineering
      Databases: 2
      Observability: 2
      Distributed Systems: 1

    section Architect
      Internet Architecture: 1
      Platform Engineering: 1
      Global Infrastructure: 1
```

---

# Repository Architecture

```mermaid
mindmap
  root((Linux Engineering Handbook))

    Foundations

      Linux Introduction
      Linux Philosophy
      Open Source
      GNU

    Core Linux

      Architecture
      Filesystems
      Processes
      Memory
      Networking
      Storage
      Security

    Administration

      Users
      Permissions
      systemd
      Bash

    Operations

      Monitoring
      Troubleshooting
      Incident Response

    Containers

      Docker
      Container Internals

    Orchestration

      Kubernetes
      Service Discovery
      Scheduling

    Cloud

      Compute
      Networking
      Storage

    Data

      Databases
      Replication
      Sharding

    Observability

      Metrics
      Logs
      Traces

    Distributed Systems

      CAP
      Consensus
      Messaging

    Internet

      DNS
      BGP
      CDN
      TLS

    Platform Engineering

      CI/CD
      GitOps
      Infrastructure as Code
```

---

# Section 1: Linux Foundations

---

## Visual Overview

```mermaid
graph TD

FOUNDATION["Linux Foundations"]

FOUNDATION --> HISTORY["History"]

FOUNDATION --> PHILOSOPHY["Philosophy"]

FOUNDATION --> GNU["GNU"]

FOUNDATION --> OPENSOURCE["Open Source"]

FOUNDATION --> POSIX["POSIX"]
```

---

## Files

```text
01-linux-introduction/

├── what-is-linux.md
├── history-of-linux.md
├── linux-philosophy.md
├── open-source-movement.md
├── gnu-and-linux.md
├── linux-distributions.md
├── why-linux-dominates-infrastructure.md
├── linux-engineering-mindset.md
└── references.md
```

---

# Section 2: Linux Architecture

---

## Architecture Map

```mermaid
graph TD

LINUX["Linux"]

LINUX --> KERNEL["Kernel"]

LINUX --> USERSPACE["User Space"]

KERNEL --> PROCESS["Processes"]

KERNEL --> MEMORY["Memory"]

KERNEL --> NETWORK["Networking"]

KERNEL --> STORAGE["Storage"]
```

---

## Files

```text
02-linux-architecture/

├── linux-architecture.md
├── linux-boot-process.md
├── linux-kernel-architecture.md
├── linux-filesystem-architecture.md
├── linux-process-lifecycle.md
├── linux-memory-management.md
├── linux-networking-stack.md
├── linux-storage-stack.md
├── linux-security-model.md
├── linux-systemd-architecture.md
└── linux-container-architecture.md
```

---

# Section 3: Filesystems

---

## Filesystem Map

```mermaid
mindmap
  root((Filesystem))

    Directories

    Files

    Inodes

    Links

      Hard
      Symbolic

    Mounts

    VFS

    ext4

    XFS

    Btrfs
```

---

## Learning Goal

Understand:

```text
How Linux stores data

How files become blocks

How inodes work

How storage reaches hardware
```

---

# Section 4: Processes

---

## Process Map

```mermaid
mindmap
  root((Processes))

    PID

    Threads

    Scheduling

    Signals

    Context Switches

    Daemons

    systemd
```

---

## Learning Goal

Understand:

```text
How programs become processes

How CPUs execute workloads

How Linux schedules work
```

---

# Section 5: Memory

---

## Memory Map

```mermaid
mindmap
  root((Memory))

    RAM

    Virtual Memory

    Paging

    Swap

    Page Cache

    OOM Killer

    NUMA
```

---

# Section 6: Networking

---

## Networking Map

```mermaid
mindmap
  root((Networking))

    TCP

    UDP

    IP

    DNS

    Routing

    Firewalls

    VPN

    Load Balancers
```

---

# Section 7: Storage

---

## Storage Map

```mermaid
mindmap
  root((Storage))

    Block Devices

    RAID

    LVM

    Filesystems

    SSD

    NVMe

    SAN

    NAS
```

---

# Section 8: Security

---

## Security Map

```mermaid
mindmap
  root((Security))

    Users

    Groups

    Permissions

    ACLs

    SELinux

    AppArmor

    Encryption
```

---

# Section 9: systemd

---

## systemd Map

```mermaid
mindmap
  root((systemd))

    PID 1

    Services

    Targets

    Timers

    Sockets

    Journald

    cgroups
```

---

# Section 10: Bash

---

## Bash Map

```mermaid
mindmap
  root((Bash))

    Variables

    Loops

    Functions

    Pipes

    Redirection

    sed

    awk

    grep
```

---

# Section 11: Troubleshooting

---

## Troubleshooting Flow

```mermaid
flowchart TD

PROBLEM["Problem"]

PROBLEM --> CPU["CPU"]

PROBLEM --> MEMORY["Memory"]

PROBLEM --> NETWORK["Network"]

PROBLEM --> STORAGE["Storage"]

PROBLEM --> APP["Application"]
```

---

## Files

```text
troubleshooting/

├── troubleshooting-decision-trees.md
├── production-incident-flows.md
├── cpu-pressure.md
├── memory-pressure.md
├── disk-full.md
├── network-failures.md
├── database-incidents.md
└── production-playbooks.md
```

---

# Section 12: Docker

---

## Docker Map

```mermaid
mindmap
  root((Docker))

    CLI

    dockerd

    containerd

    runc

    Namespaces

    cgroups

    OverlayFS

    Images

    Volumes

    Networking
```

---

# Section 13: Kubernetes

---

## Kubernetes Map

```mermaid
mindmap
  root((Kubernetes))

    API Server

    Scheduler

    Controllers

    etcd

    Nodes

    Pods

    Services

    Ingress

    Storage
```

---

# Section 14: Cloud

---

## Cloud Map

```mermaid
mindmap
  root((Cloud))

    Datacenters

    Linux

    Hypervisors

    Virtual Machines

    Containers

    Kubernetes

    Networking

    Storage
```

---

# Section 15: Databases

---

## Database Map

```mermaid
mindmap
  root((Databases))

    Query Engine

    Optimizer

    Indexes

    WAL

    MVCC

    Replication

    Sharding
```

---

# Section 16: Observability

---

## Observability Map

```mermaid
mindmap
  root((Observability))

    Metrics

    Logs

    Traces

    Events

    Alerting

    OpenTelemetry

    eBPF
```

---

# Section 17: Distributed Systems

---

## Distributed Systems Map

```mermaid
mindmap
  root((Distributed Systems))

    Replication

    Consensus

    CAP

    PACELC

    Kafka

    Service Discovery

    Multi Region
```

---

# Section 18: Internet Architecture

---

## Internet Map

```mermaid
mindmap
  root((Internet))

    DNS

    BGP

    TCP

    UDP

    TLS

    CDN

    Load Balancers

    Cloud
```

---

# Section 19: Platform Engineering

---

## Platform Map

```mermaid
mindmap
  root((Platform Engineering))

    CI/CD

    GitOps

    Terraform

    Internal Platforms

    Developer Experience

    Automation
```

---

# Section 20: Founder-Level Systems Thinking

---

## Infrastructure Evolution

```mermaid
flowchart TD

LINUX["Linux Server"]

LINUX --> CONTAINERS["Containers"]

CONTAINERS --> K8S["Kubernetes"]

K8S --> CLOUD["Cloud"]

CLOUD --> DATABASES["Databases"]

DATABASES --> DISTRIBUTED["Distributed Systems"]

DISTRIBUTED --> INTERNET["Internet Scale"]

INTERNET --> PLATFORM["Platform Engineering"]

PLATFORM --> BUSINESS["Technology Business"]
```

---

# Visual Dependency Graph

This shows how every topic depends on another.

```mermaid
graph LR

LINUX["Linux"]

LINUX --> PROCESS["Processes"]

LINUX --> MEMORY["Memory"]

LINUX --> FS["Filesystem"]

LINUX --> NET["Networking"]

PROCESS --> CONTAINER["Containers"]

CONTAINER --> K8S["Kubernetes"]

K8S --> CLOUD["Cloud"]

FS --> DATABASE["Databases"]

NET --> INTERNET["Internet"]

DATABASE --> DIST["Distributed Systems"]

DIST --> PLATFORM["Platforms"]

PLATFORM --> SRE["SRE"]

SRE --> ARCH["Architecture"]
```

---

# Repository Completion Map

```mermaid
mindmap
  root((Linux Mastery))

    Beginner

      Commands
      Files
      Navigation

    Intermediate

      Processes
      Networking
      Storage

    Advanced

      Security
      Performance
      Troubleshooting

    Production

      Docker
      Kubernetes
      Databases

    Infrastructure

      Cloud
      Observability

    Distributed Systems

      Replication
      Consensus

    Architect

      Internet
      Platforms
      Global Systems
```

---

# Final Takeaway

This repository is not a collection of notes.

It is a complete engineering journey.

```text
Linux
   ↓
Architecture
   ↓
Processes
   ↓
Networking
   ↓
Storage
   ↓
Security
   ↓
Containers
   ↓
Kubernetes
   ↓
Cloud
   ↓
Databases
   ↓
Observability
   ↓
Distributed Systems
   ↓
Internet
   ↓
Platform Engineering
   ↓
System Architecture
```

Use this file as the central navigation hub for the entire Linux Engineering Handbook.
