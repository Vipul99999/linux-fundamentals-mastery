# Linux Container Architecture

## From Namespaces and cgroups to Docker, Kubernetes, and Modern Cloud Infrastructure

---

# Why This Exists

Containers changed software engineering.

Before containers:

```text
Developer Machine
        ≠
Testing Environment
        ≠
Production Environment
```

Teams constantly heard:

```text
"It works on my machine."
```

Containers solved this problem by packaging:

```text
Application
+
Dependencies
+
Runtime
+
Configuration
```

into a portable unit.

However, one of the biggest misconceptions in technology is:

> Containers are not virtual machines.

Containers are fundamentally Linux kernel features.

Understanding container architecture is essential for:

* Linux Engineers
* Backend Engineers
* DevOps Engineers
* Cloud Engineers
* SREs
* Platform Engineers
* Kubernetes Engineers
* Infrastructure Architects

---

# The Container Mental Model

Most beginners think:

```text
Container
     ↓
Mini Virtual Machine
```

Wrong.

Reality:

```text
Container
     ↓
Isolated Linux Process
```

Containers share the host kernel.

Think of a container as:

```text
A private apartment
inside a shared building.
```

Each apartment appears independent.

But all apartments share:

```text
Foundation
Electricity
Water
Building Structure
```

Likewise containers share:

```text
Linux Kernel
```

---

# The Big Picture

```mermaid
graph TD

APP["Application"]

APP --> CONTAINER["Container"]

CONTAINER --> NAMESPACE["Namespaces"]

CONTAINER --> CGROUP["cgroups"]

CONTAINER --> ROOTFS["Root Filesystem"]

NAMESPACE --> KERNEL["Linux Kernel"]

CGROUP --> KERNEL

ROOTFS --> KERNEL
```

---

# Container Architecture Overview

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> PROCESS["Processes"]

CONTAINER --> NETWORK["Network"]

CONTAINER --> FILESYSTEM["Filesystem"]

CONTAINER --> USERS["Users"]

PROCESS --> NAMESPACE["Namespaces"]

NETWORK --> NAMESPACE

FILESYSTEM --> NAMESPACE

USERS --> NAMESPACE

CONTAINER --> CGROUP["cgroups"]

CONTAINER --> IMAGE["Container Image"]
```

---

# Containers Are Linux Features

Containers are built from:

```text
Namespaces
cgroups
Capabilities
seccomp
OverlayFS
Virtual Ethernet
```

There is no:

```text
Container Magic
```

Only Linux primitives.

---

# Traditional Server Architecture

Before containers:

```mermaid
graph TD

SERVER["Server"]

SERVER --> APP1["Application A"]

SERVER --> APP2["Application B"]

SERVER --> APP3["Application C"]

APP1 --> LIB1["Library Version 1"]

APP2 --> LIB2["Library Version 2"]

APP3 --> LIB3["Library Version 3"]
```

Dependency conflicts become common.

---

# Containerized Architecture

```mermaid
graph TD

SERVER["Linux Host"]

SERVER --> C1["Container A"]

SERVER --> C2["Container B"]

SERVER --> C3["Container C"]

C1 --> APP1["App A"]

C2 --> APP2["App B"]

C3 --> APP3["App C"]
```

Isolation improves consistency.

---

# Containers vs Virtual Machines

## Virtual Machine Architecture

```mermaid
graph TD

APP1["Application"]

APP1 --> GUEST1["Guest OS"]

GUEST1 --> HYP["Hypervisor"]

HYP --> HARDWARE["Hardware"]
```

---

## Container Architecture

```mermaid
graph TD

APP["Application"]

APP --> CONTAINER["Container"]

CONTAINER --> KERNEL["Shared Linux Kernel"]

KERNEL --> HARDWARE["Hardware"]
```

---

# Key Difference

VM:

```text
Own Kernel
```

Container:

```text
Shared Kernel
```

---

# Resource Comparison

| Feature   | Container     | VM       |
| --------- | ------------- | -------- |
| Boot Time | Seconds       | Minutes  |
| Memory    | Low           | High     |
| Isolation | Process Level | OS Level |
| Kernel    | Shared        | Separate |
| Density   | Very High     | Lower    |

---

# Namespace Architecture

Namespaces create isolation.

---

# Namespace Mental Model

Each container sees:

```text
Its Own World
```

even though resources are shared.

---

# Namespace Overview

```mermaid
graph TD

HOST["Host"]

HOST --> PID["PID Namespace"]

HOST --> NET["Network Namespace"]

HOST --> MOUNT["Mount Namespace"]

HOST --> IPC["IPC Namespace"]

HOST --> USER["User Namespace"]

HOST --> UTS["UTS Namespace"]
```

---

# PID Namespace

Provides process isolation.

---

# PID Namespace Example

Host:

```text
PID 1
PID 200
PID 500
PID 1000
```

Container:

```text
PID 1
PID 2
PID 3
```

Same process.

Different views.

---

# PID Namespace Architecture

```mermaid
graph TD

HOST["Host Processes"]

HOST --> CONTAINER["Container"]

CONTAINER --> PID1["PID 1"]

CONTAINER --> PID2["PID 2"]

CONTAINER --> PID3["PID 3"]
```

---

# Network Namespace

Provides separate networking.

Each container gets:

```text
Interfaces

Routing Table

ARP Table

Firewall Rules
```

---

# Network Namespace Architecture

```mermaid
graph TD

HOST["Host"]

HOST --> NS1["Namespace A"]

HOST --> NS2["Namespace B"]

NS1 --> IP1["10.0.0.2"]

NS2 --> IP2["10.0.0.3"]
```

---

# Mount Namespace

Provides filesystem isolation.

---

# Mount Architecture

```mermaid
graph TD

HOST["Host Filesystem"]

HOST --> NS1["Container Filesystem"]

HOST --> NS2["Container Filesystem"]
```

---

# UTS Namespace

Provides hostname isolation.

---

# Example

Container A:

```text
web-1
```

Container B:

```text
db-1
```

Same host.

Different hostnames.

---

# IPC Namespace

Isolates:

```text
Shared Memory

Message Queues

Semaphores
```

---

# User Namespace

Maps container users to host users.

Important for security.

---

# User Namespace Architecture

```mermaid
graph TD

CONTAINERROOT["Container Root"]

CONTAINERROOT --> HOSTUSER["Host User 100000"]
```

Container root is not necessarily host root.

---

# cgroups

Namespaces isolate visibility.

cgroups control resources.

---

# cgroup Architecture

```mermaid
graph TD

CGROUP["cgroup"]

CGROUP --> CPU["CPU Limit"]

CGROUP --> MEM["Memory Limit"]

CGROUP --> IO["I/O Limit"]

CGROUP --> NET["Network Limit"]
```

---

# Why cgroups Exist

Without cgroups:

```text
One Container
       ↓
Consumes Entire Server
```

With cgroups:

```text
Resource Limits
```

---

# CPU Limits

```mermaid
graph TD

SERVER["CPU"]

SERVER --> C1["Container A 50%"]

SERVER --> C2["Container B 25%"]

SERVER --> C3["Container C 25%"]
```

---

# Memory Limits

```mermaid
graph TD

HOST["Host RAM"]

HOST --> C1["512MB"]

HOST --> C2["1GB"]

HOST --> C3["2GB"]
```

---

# OOM Flow

```mermaid
flowchart TD

CONTAINER["Container"]

CONTAINER --> LIMIT["Memory Limit"]

LIMIT --> EXCEEDED["Limit Exceeded"]

EXCEEDED --> OOM["OOM Kill"]
```

---

# Container Filesystems

Containers use layered filesystems.

---

# OverlayFS Architecture

```mermaid
graph TD

BASE["Base Image"]

BASE --> LAYER1["Layer 1"]

LAYER1 --> LAYER2["Layer 2"]

LAYER2 --> RW["Writable Layer"]
```

---

# Why Layers Exist

Benefits:

```text
Reuse

Caching

Efficiency

Fast Deployment
```

---

# Container Image Architecture

```mermaid
graph TD

IMAGE["Container Image"]

IMAGE --> OS["Base OS"]

IMAGE --> LIBS["Libraries"]

IMAGE --> RUNTIME["Runtime"]

IMAGE --> APP["Application"]
```

---

# Docker Architecture

Docker is a container platform.

Containers existed before Docker.

Docker made them easy.

---

# Docker Stack

```mermaid
graph TD

CLI["Docker CLI"]

CLI --> DOCKERD["dockerd"]

DOCKERD --> CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> CONTAINER["Container"]
```

---

# Container Runtime Flow

```mermaid
flowchart TD

IMAGE["Container Image"]

IMAGE --> CREATE["Create Container"]

CREATE --> NAMESPACE["Namespaces"]

NAMESPACE --> CGROUP["cgroups"]

CGROUP --> PROCESS["Application Process"]
```

---

# Container Lifecycle

```mermaid
stateDiagram-v2

Created --> Running

Running --> Paused

Paused --> Running

Running --> Stopped

Stopped --> Deleted
```

---

# What Actually Runs?

A container is usually:

```text
One Main Process
```

Example:

```text
nginx
postgres
redis
node
python
```

---

# Container Process Architecture

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> PID1["Main Process"]

PID1 --> WORKERS["Worker Processes"]
```

---

# Networking Architecture

Docker networking uses:

```text
veth pairs

bridges

NAT
```

---

# Docker Network Flow

```mermaid
graph LR

CONTAINER["Container"]

CONTAINER --> VETH["veth"]

VETH --> BRIDGE["docker0"]

BRIDGE --> HOST["Host"]

HOST --> INTERNET["Internet"]
```

---

# Container Storage

Persistent storage uses:

```text
Volumes

Bind Mounts

Network Storage
```

---

# Volume Architecture

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> VOLUME["Volume"]

VOLUME --> HOST["Host Storage"]
```

---

# Security Architecture

Containers use:

```text
Namespaces

Capabilities

seccomp

AppArmor

SELinux
```

---

# Security Layers

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> NAMESPACE["Namespaces"]

CONTAINER --> CAPS["Capabilities"]

CONTAINER --> SECCOMP["seccomp"]

CONTAINER --> SELINUX["SELinux"]
```

---

# seccomp

Restricts system calls.

---

# seccomp Flow

```mermaid
graph TD

PROCESS["Process"]

PROCESS --> SYSCALL["System Call"]

SYSCALL --> FILTER["seccomp"]

FILTER --> ALLOW["Allow"]

FILTER --> BLOCK["Block"]
```

---

# Container Observability

Important metrics:

```text
CPU

Memory

Network

Storage

Processes
```

---

# Monitoring Architecture

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> METRICS["Metrics"]

CONTAINER --> LOGS["Logs"]

CONTAINER --> EVENTS["Events"]
```

---

# Kubernetes Foundation

Kubernetes orchestrates containers.

---

# Kubernetes Architecture

```mermaid
graph TD

POD["Pod"]

POD --> CONTAINER1["Container"]

POD --> CONTAINER2["Container"]

CONTAINER1 --> KERNEL["Linux Kernel"]

CONTAINER2 --> KERNEL
```

---

# Pod Networking

```mermaid
graph TD

POD["Pod"]

POD --> IP["Pod IP"]

IP --> CLUSTER["Cluster Network"]
```

---

# Container Startup Flow

```mermaid
flowchart TD

IMAGE["Image"]

IMAGE --> PULL["Pull"]

PULL --> CREATE["Create"]

CREATE --> NS["Namespaces"]

NS --> CG["cgroups"]

CG --> START["Start Process"]

START --> RUNNING["Running"]
```

---

# Production Container Stack

```mermaid
graph TD

APPLICATION["Application"]

APPLICATION --> CONTAINER["Container"]

CONTAINER --> DOCKER["Container Runtime"]

DOCKER --> KERNEL["Linux Kernel"]

KERNEL --> CLOUD["Cloud Infrastructure"]
```

---

# Common Container Problems

```text
OOM Kills

Image Bloat

Storage Leaks

Network Issues

Container Restarts

PID 1 Problems

Permission Issues
```

---

# Troubleshooting Flow

```mermaid
flowchart TD

ISSUE["Container Problem"]

ISSUE --> CPU["CPU?"]

ISSUE --> MEM["Memory?"]

ISSUE --> NET["Network?"]

ISSUE --> STORAGE["Storage?"]

CPU --> TOP["top"]

MEM --> STATS["docker stats"]

NET --> SS["ss"]

STORAGE --> DF["df -h"]
```

---

# Engineering Mindset

Beginners see:

```text
Docker Container
```

Engineers see:

```text
Process
      ↓
Namespaces
      ↓
cgroups
      ↓
OverlayFS
      ↓
Virtual Networking
      ↓
Linux Kernel
```

Containers are Linux abstractions, not virtual machines.

---

# Interview Questions

### What is a container?

### How are containers different from VMs?

### What are namespaces?

### What are cgroups?

### What is OverlayFS?

### What is a container image?

### What is a container runtime?

### What is runc?

### What is containerd?

### How does Docker networking work?

### What is a veth pair?

### How do containers share the kernel?

### What is seccomp?

### Why do containers get OOMKilled?

### How does Kubernetes use containers?

---

# One-Page Architecture Summary

```text
Container
     ↓
Namespaces
     ↓
cgroups
     ↓
OverlayFS
     ↓
Virtual Networking
     ↓
Linux Kernel
     ↓
Hardware
```

---

# Final Takeaway

Containers are not miniature virtual machines.

They are isolated Linux processes built from powerful kernel primitives:

```text
Namespaces

cgroups

Capabilities

seccomp

OverlayFS

Virtual Networking
```

Docker, Kubernetes, and modern cloud-native platforms are all built on top of these Linux foundations.

Master container architecture and you gain the ability to understand, debug, optimize, secure, and scale modern applications from a single container to thousands of Kubernetes nodes across the globe.
