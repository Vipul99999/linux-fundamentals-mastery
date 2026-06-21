# Architecture Diagrams

> Great engineers do not memorize systems.

> Great engineers visualize systems.

> If you cannot draw it, you do not fully understand it.

---

# Why This Exists

Humans understand stories and pictures better than isolated facts.

Most Linux learning fails because people memorize:

```text
Commands

Configurations

Tools
```

But infrastructure is actually:

```text
Relationships

Data flow

Resources

Dependencies
```

This file converts infrastructure into pictures.

---

# Diagram 1: Linux Universe

```mermaid
flowchart TD

Users

Applications

LinuxKernel

Hardware

Users --> Applications

Applications --> LinuxKernel

LinuxKernel --> Hardware
```

Mental model:

> Linux is the translator between software and hardware.

---

# Diagram 2: Linux Internals

```mermaid
flowchart TD

Applications

Syscalls

Kernel

Scheduler

MemoryManager

VFS

NetworkStack

Drivers

Hardware

Applications --> Syscalls

Syscalls --> Kernel

Kernel --> Scheduler

Kernel --> MemoryManager

Kernel --> VFS

Kernel --> NetworkStack

Scheduler --> Drivers

MemoryManager --> Drivers

VFS --> Drivers

NetworkStack --> Drivers

Drivers --> Hardware
```

---

# Diagram 3: Request Lifecycle

```mermaid
flowchart LR

User

Browser

DNS

CDN

LoadBalancer

API

Database

Response

User --> Browser

Browser --> DNS

DNS --> CDN

CDN --> LoadBalancer

LoadBalancer --> API

API --> Database

Database --> Response
```

Question:

> Where can latency occur?

Answer:

```text
Every arrow.
```

---

# Diagram 4: Linux Process Lifecycle

```mermaid
flowchart TD

Program

fork

Process

Scheduler

CPU

Exit

Program --> fork

fork --> Process

Process --> Scheduler

Scheduler --> CPU

CPU --> Exit
```

---

# Diagram 5: Process Memory Layout

```text
High Memory

+----------------+
| Kernel Space   |
+----------------+

| Stack          |

| Shared Library |

| Heap           |

| Data Segment   |

| Text Segment   |

+----------------+

Low Memory
```

---

# Diagram 6: Syscall Journey

```mermaid
flowchart TD

Application

Syscall

Kernel

Filesystem

Driver

Hardware

Application --> Syscall

Syscall --> Kernel

Kernel --> Filesystem

Filesystem --> Driver

Driver --> Hardware
```

---

# Diagram 7: CPU Scheduling

```mermaid
flowchart TD

Processes

RunQueue

Scheduler

CPU

Processes --> RunQueue

RunQueue --> Scheduler

Scheduler --> CPU
```

---

# Diagram 8: Context Switching

```mermaid
flowchart LR

ProcessA

SaveState

Scheduler

LoadState

ProcessB

ProcessA --> SaveState

SaveState --> Scheduler

Scheduler --> LoadState

LoadState --> ProcessB
```

---

# Diagram 9: Memory Hierarchy

```text
Fast

CPU Register

↓

L1 Cache

↓

L2 Cache

↓

L3 Cache

↓

RAM

↓

SSD

↓

HDD

↓

Internet

Slow
```

---

# Diagram 10: Linux Memory Management

```mermaid
flowchart TD

Application

VirtualMemory

PhysicalMemory

PageCache

Swap

Disk

Application --> VirtualMemory

VirtualMemory --> PhysicalMemory

PhysicalMemory --> PageCache

PhysicalMemory --> Swap

Swap --> Disk
```

---

# Diagram 11: OOM Killer Decision

```mermaid
flowchart TD

MemoryPressure

OOM

Score

KillProcess

Recover

MemoryPressure --> OOM

OOM --> Score

Score --> KillProcess

KillProcess --> Recover
```

---

# Diagram 12: Linux Storage Pipeline

```mermaid
flowchart TD

Application

VFS

Filesystem

PageCache

BlockLayer

IOScheduler

Driver

Disk

Application --> VFS

VFS --> Filesystem

Filesystem --> PageCache

PageCache --> BlockLayer

BlockLayer --> IOScheduler

IOScheduler --> Driver

Driver --> Disk
```

---

# Diagram 13: Network Stack

```mermaid
flowchart TD

Application

Socket

TCP

IP

NIC

Wire

Application --> Socket

Socket --> TCP

TCP --> IP

IP --> NIC

NIC --> Wire
```

---

# Diagram 14: TCP Connection

```mermaid
sequenceDiagram

Client->>Server: SYN

Server->>Client: SYN-ACK

Client->>Server: ACK
```

---

# Diagram 15: Docker Architecture

```mermaid
flowchart TD

Container

Namespaces

cgroups

LinuxKernel

Hardware

Container --> Namespaces

Namespaces --> cgroups

cgroups --> LinuxKernel

LinuxKernel --> Hardware
```

---

# Diagram 16: Container Internals

```mermaid
flowchart TD

Application

Container

OverlayFS

Linux

Disk

Application --> Container

Container --> OverlayFS

OverlayFS --> Linux

Linux --> Disk
```

---

# Diagram 17: Kubernetes Architecture

```mermaid
flowchart TD

Users

Ingress

Service

Pods

Containers

Linux

Users --> Ingress

Ingress --> Service

Service --> Pods

Pods --> Containers

Containers --> Linux
```

---

# Diagram 18: Kubernetes Cluster

```mermaid
flowchart TD

ControlPlane

Worker1

Worker2

Worker3

ControlPlane --> Worker1

ControlPlane --> Worker2

ControlPlane --> Worker3
```

---

# Diagram 19: Production Architecture

```mermaid
flowchart TD

Users

CDN

LoadBalancer

API

Redis

Database

Storage

Users --> CDN

CDN --> LoadBalancer

LoadBalancer --> API

API --> Redis

Redis --> Database

Database --> Storage
```

---

# Diagram 20: Caching Hierarchy

```text
CPU Cache

↓

Linux Page Cache

↓

Redis

↓

CDN

↓

Database
```

Every layer is a cache.

---

# Diagram 21: Queueing System

```mermaid
flowchart TD

Users

Queue

Workers

Database

Users --> Queue

Queue --> Workers

Workers --> Database
```

---

# Diagram 22: Event Driven Architecture

```mermaid
flowchart TD

Producer

Queue

Consumers

Producer --> Queue

Queue --> Consumers
```

---

# Diagram 23: Microservice Architecture

```mermaid
flowchart TD

Gateway

Auth

Users

Payments

Notifications

Gateway --> Auth

Gateway --> Users

Gateway --> Payments

Gateway --> Notifications
```

---

# Diagram 24: Database Replication

```mermaid
flowchart TD

Primary

Replica1

Replica2

Replica3

Primary --> Replica1

Primary --> Replica2

Primary --> Replica3
```

---

# Diagram 25: Database Sharding

```mermaid
flowchart TD

Users

ShardA

ShardB

ShardC

Users --> ShardA

Users --> ShardB

Users --> ShardC
```

---

# Diagram 26: Load Balancer

```mermaid
flowchart TD

Users

LoadBalancer

Server1

Server2

Server3

Users --> LoadBalancer

LoadBalancer --> Server1

LoadBalancer --> Server2

LoadBalancer --> Server3
```

---

# Diagram 27: Circuit Breaker

```mermaid
flowchart TD

Closed

Open

HalfOpen

Closed --> Open

Open --> HalfOpen

HalfOpen --> Closed
```

---

# Diagram 28: Retry Storm

```mermaid
flowchart TD

SlowService

Retries

Traffic

Collapse

SlowService --> Retries

Retries --> Traffic

Traffic --> Collapse
```

---

# Diagram 29: Scaling Journey

```mermaid
flowchart LR

SingleServer

VerticalScaling

HorizontalScaling

DistributedSystems

GlobalSystems

SingleServer --> VerticalScaling

VerticalScaling --> HorizontalScaling

HorizontalScaling --> DistributedSystems

DistributedSystems --> GlobalSystems
```

---

# Diagram 30: Observability Pillars

```mermaid
flowchart TD

Infrastructure

Metrics

Logs

Traces

Profiles

Infrastructure --> Metrics

Infrastructure --> Logs

Infrastructure --> Traces

Infrastructure --> Profiles
```

---

# Diagram 31: The Four Golden Signals

```mermaid
flowchart TD

Latency

Traffic

Errors

Saturation
```

Monitor these everywhere.

---

# Diagram 32: Linux Powers Everything

```mermaid
flowchart TD

AI

Cloud

Kubernetes

Docker

Databases

Linux

Hardware

AI --> Linux

Cloud --> Linux

Kubernetes --> Linux

Docker --> Linux

Databases --> Linux

Linux --> Hardware
```

---

# Diagram 33: The Universal Bottleneck Formula

```text
Demand

↓

Capacity

↓

Queues

↓

Latency

↓

Timeouts

↓

Failures
```

This explains most outages.

---

# Diagram 34: Production Troubleshooting Flow

```mermaid
flowchart TD

Symptoms

Metrics

Resources

Bottleneck

RootCause

Fix

Symptoms --> Metrics

Metrics --> Resources

Resources --> Bottleneck

Bottleneck --> RootCause

RootCause --> Fix
```

---

# Diagram 35: Infrastructure Evolution

```text
Laptop

↓

Server

↓

Cluster

↓

Distributed System

↓

Cloud

↓

Planetary Infrastructure
```

---

# Diagram 36: The Modern Technology Stack

```mermaid
flowchart TD

Users

Frontend

API

Cache

Database

Linux

Hardware

Users --> Frontend

Frontend --> API

API --> Cache

Cache --> Database

Database --> Linux

Linux --> Hardware
```

---

# Diagram 37: Systems Thinking Diagram

```mermaid
flowchart TD

Users

Requests

Applications

Resources

Failures

Users --> Requests

Requests --> Applications

Applications --> Resources

Resources --> Failures
```

Everything is connected.

---

# Diagram 38: The Engineer Evolution Journey

```text
Commands

↓

Tools

↓

Systems

↓

Patterns

↓

Architecture

↓

Engineering Thinking
```

---

# Diagram 39: The Linux Engineering Universe

```mermaid
mindmap

root((Linux Engineering))

Processes

CPU

Memory

Storage

Networking

Security

Performance

Reliability

Containers

Kubernetes

Cloud

Databases

Distributed Systems

SRE

Platform Engineering
```

---

# Diagram 40: The Entire Modern Internet

```mermaid
flowchart TD

Users

DNS

CDN

LoadBalancer

Microservices

Caches

Databases

Linux

Hardware

Users --> DNS

DNS --> CDN

CDN --> LoadBalancer

LoadBalancer --> Microservices

Microservices --> Caches

Caches --> Databases

Databases --> Linux

Linux --> Hardware
```

---

# Engineering Mindset

Do not memorize diagrams.

Ask three questions for every diagram:

```text
Who creates the data?

Who moves the data?

Who becomes the bottleneck?
```

---

# Cheat Sheet

```text
If you can draw it

↓

You can understand it

↓

You can troubleshoot it

↓

You can optimize it

↓

You can scale it

↓

You can build it
```

---

# Final Thought

The biggest difference between beginners and architects is not knowledge.

It is visualization.

Beginners see:

```text
Tools
```

Architects see:

```text
Relationships
```

Because modern infrastructure is simply millions of moving pieces that engineers have learned how to visualize.
