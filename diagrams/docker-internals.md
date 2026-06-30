# Docker Internals

## From `docker run` to Linux Namespaces, cgroups, OverlayFS, containerd, and runc

---

# Why This Exists

Most engineers use Docker every day.

They run commands like:

```bash
docker run nginx
docker ps
docker build .
docker compose up
```

and Docker appears to magically create containers.

But Docker is not magic.

When you execute:

```bash
docker run nginx
```

Docker does not create a virtual machine.

Docker does not create a new operating system.

Docker ultimately creates:

```text
Linux Process
+
Namespaces
+
cgroups
+
Filesystem Layers
+
Networking
```

Understanding Docker internals is important because:

* Containers are the foundation of Kubernetes
* Modern cloud platforms run containers
* Production debugging often requires Linux-level understanding
* Performance tuning requires understanding what Docker actually does

---

# The Core Mental Model

Most beginners think:

```text
Application
     ↓
Docker
     ↓
Container
```

Reality:

```text
Application
     ↓
Docker
     ↓
containerd
     ↓
runc
     ↓
Linux Kernel
     ↓
Namespaces
     ↓
cgroups
     ↓
Process
```

A container is ultimately:

```text
An isolated Linux process.
```

---

# The Big Picture

```mermaid
graph TD

CLI["docker CLI"]

CLI --> DOCKERD["dockerd"]

DOCKERD --> CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> NAMESPACE["Namespaces"]

RUNC --> CGROUP["cgroups"]

RUNC --> OVERLAY["OverlayFS"]

NAMESPACE --> PROCESS["Container Process"]

CGROUP --> PROCESS

OVERLAY --> PROCESS
```

---

# Docker Architecture Overview

```mermaid
graph TD

DOCKER["Docker Platform"]

DOCKER --> CLI["Docker CLI"]

DOCKER --> DAEMON["dockerd"]

DOCKER --> CONTAINERD["containerd"]

DOCKER --> RUNTIME["runc"]

DOCKER --> IMAGES["Images"]

DOCKER --> NETWORK["Networking"]

DOCKER --> STORAGE["Storage"]
```

---

# Docker Components

Modern Docker consists of:

```text
Docker CLI

dockerd

containerd

runc

Linux Kernel
```

Each has a specific responsibility.

---

# Docker CLI

The command-line interface.

Example:

```bash
docker run nginx
```

The CLI itself does not create containers.

It sends API requests.

---

# CLI Architecture

```mermaid
graph LR

USER["User"]

USER --> CLI["Docker CLI"]

CLI --> API["Docker API"]
```

---

# dockerd

Docker daemon.

Responsible for:

```text
Image Management

Networking

Volume Management

Container Lifecycle

API Management
```

---

# dockerd Architecture

```mermaid
graph TD

CLI["Docker CLI"]

CLI --> DOCKERD["dockerd"]

DOCKERD --> IMAGES["Images"]

DOCKERD --> NETWORK["Networks"]

DOCKERD --> VOLUMES["Volumes"]

DOCKERD --> CONTAINERS["Containers"]
```

---

# containerd

A container lifecycle manager.

Docker delegates container management to containerd.

---

# Why containerd Exists

Historically:

```text
Docker Did Everything
```

Modern architecture:

```text
Docker
   ↓
containerd
   ↓
runc
```

This separation improves modularity.

---

# containerd Responsibilities

```text
Image Pulling

Container Lifecycle

Snapshots

Runtime Management
```

---

# containerd Architecture

```mermaid
graph TD

DOCKERD["dockerd"]

DOCKERD --> CONTAINERD["containerd"]

CONTAINERD --> SNAPSHOTS["Snapshots"]

CONTAINERD --> IMAGES["Images"]

CONTAINERD --> TASKS["Container Tasks"]
```

---

# runc

The actual low-level runtime.

---

# What runc Does

runc directly interacts with Linux kernel features.

Creates:

```text
Namespaces

cgroups

Container Process
```

---

# runc Architecture

```mermaid
graph TD

CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> KERNEL["Linux Kernel"]

KERNEL --> PROCESS["Container Process"]
```

---

# The Complete Container Creation Flow

When you run:

```bash
docker run nginx
```

---

# Internal Flow

```mermaid
sequenceDiagram

User->>CLI: docker run nginx

CLI->>dockerd: Create Container

dockerd->>containerd: Start Container

containerd->>runc: Create Runtime

runc->>Kernel: Create Namespaces

runc->>Kernel: Create cgroups

runc->>Kernel: Launch Process

Kernel-->>User: Running Container
```

---

# Container = Process

The most important concept.

Container:

```text
Not VM

Not OS

Not Hypervisor
```

Container:

```text
Linux Process
```

---

# Verify This Yourself

Run:

```bash
docker run -d nginx
```

Then:

```bash
ps aux
```

You'll see Linux processes.

---

# Namespaces

Namespaces provide isolation.

---

# Namespace Types

```text
PID

Network

Mount

IPC

UTS

User
```

---

# Namespace Architecture

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> PID["PID Namespace"]

CONTAINER --> NET["Network Namespace"]

CONTAINER --> MOUNT["Mount Namespace"]

CONTAINER --> IPC["IPC Namespace"]

CONTAINER --> USER["User Namespace"]
```

---

# PID Namespace

Creates process isolation.

Container sees:

```text
PID 1
PID 2
PID 3
```

Host sees:

```text
PID 2001
PID 2002
PID 2003
```

Same process.

Different view.

---

# PID Namespace Visualization

```mermaid
graph TD

HOST["Host"]

HOST --> PID2001["PID 2001"]

HOST --> PID2002["PID 2002"]

PID2001 --> CONTAINER["Container PID 1"]
```

---

# Network Namespace

Each container gets:

```text
Network Interfaces

Routing Table

ARP Table

Firewall Rules
```

---

# Network Namespace Architecture

```mermaid
graph TD

HOST["Host"]

HOST --> NS1["Container A"]

HOST --> NS2["Container B"]

NS1 --> IP1["172.17.0.2"]

NS2 --> IP2["172.17.0.3"]
```

---

# Mount Namespace

Provides filesystem isolation.

---

# Mount Namespace Architecture

```mermaid
graph TD

HOST["Host Filesystem"]

HOST --> C1["Container Filesystem"]

HOST --> C2["Container Filesystem"]
```

---

# UTS Namespace

Provides hostname isolation.

---

# Example

Container A:

```text
web
```

Container B:

```text
db
```

Different hostnames.

---

# User Namespace

Maps container users to host users.

---

# Security Benefit

Container root:

```text
UID 0
```

can map to:

```text
UID 100000
```

on host.

---

# cgroups

Namespaces isolate visibility.

cgroups limit resources.

---

# cgroup Architecture

```mermaid
graph TD

CONTAINER["Container"]

CONTAINER --> CPU["CPU Limits"]

CONTAINER --> MEM["Memory Limits"]

CONTAINER --> IO["I/O Limits"]

CONTAINER --> PID["PID Limits"]
```

---

# Why cgroups Exist

Without limits:

```text
One Container
     ↓
Consumes Entire Server
```

---

# CPU Limits

Example:

```bash
docker run --cpus=2 nginx
```

Linux enforces via cgroups.

---

# Memory Limits

Example:

```bash
docker run -m 512m nginx
```

---

# Memory Flow

```mermaid
flowchart TD

CONTAINER["Container"]

CONTAINER --> LIMIT["512 MB"]

LIMIT --> OOM["OOM Kill"]
```

---

# OOM Kills

If memory limit exceeded:

```text
Linux OOM Killer
```

terminates process.

---

# Filesystem Isolation

Containers require isolated filesystems.

Docker uses:

```text
OverlayFS
```

---

# OverlayFS Architecture

```mermaid
graph TD

IMAGE["Image"]

IMAGE --> LAYER1["Layer"]

LAYER1 --> LAYER2["Layer"]

LAYER2 --> RW["Writable Layer"]
```

---

# Why Layers Exist

Benefits:

```text
Reuse

Caching

Fast Pulls

Reduced Storage
```

---

# Container Image Architecture

```mermaid
graph TD

IMAGE["Docker Image"]

IMAGE --> OS["Base OS"]

IMAGE --> LIBS["Libraries"]

IMAGE --> APP["Application"]
```

---

# Image Build Process

```mermaid
flowchart TD

DOCKERFILE["Dockerfile"]

DOCKERFILE --> LAYER1["Layer"]

LAYER1 --> LAYER2["Layer"]

LAYER2 --> IMAGE["Final Image"]
```

---

# Example Layering

```dockerfile
FROM ubuntu

RUN apt install nginx

COPY . .

CMD nginx
```

Creates multiple layers.

---

# Copy-On-Write

Containers share image layers.

Only changed data is stored separately.

---

# Copy-On-Write Architecture

```mermaid
graph TD

IMAGE["Read Only Layers"]

IMAGE --> RW["Writable Layer"]

RW --> CONTAINER["Running Container"]
```

---

# Docker Networking

One of Docker's most important internals.

---

# Default Network Architecture

```mermaid
graph LR

CONTAINER["Container"]

CONTAINER --> VETH["veth Pair"]

VETH --> BRIDGE["docker0"]

BRIDGE --> HOST["Host"]

HOST --> INTERNET["Internet"]
```

---

# Docker Networking Components

```text
veth Pair

Bridge

NAT

iptables
```

---

# veth Pair

Acts like a virtual cable.

---

# Visualization

```mermaid
graph LR

CONTAINER["Container"]

CONTAINER --> VETH1["veth"]

VETH1 --> VETH2["veth"]

VETH2 --> HOST["Host"]
```

---

# docker0 Bridge

Default virtual switch.

---

# Bridge Architecture

```mermaid
graph TD

CONTAINER1["Container A"]

CONTAINER2["Container B"]

CONTAINER3["Container C"]

CONTAINER1 --> BRIDGE["docker0"]

CONTAINER2 --> BRIDGE

CONTAINER3 --> BRIDGE
```

---

# NAT

Containers typically use private IPs.

Linux performs:

```text
Network Address Translation
```

for external access.

---

# Docker Storage

Persistent storage options:

```text
Volumes

Bind Mounts

tmpfs
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

# Logging

Container stdout/stderr becomes logs.

---

# Logging Flow

```mermaid
graph TD

PROCESS["Application"]

PROCESS --> STDOUT["stdout"]

STDOUT --> DOCKER["Docker Logging Driver"]

DOCKER --> STORAGE["Logs"]
```

---

# Security Model

Containers use:

```text
Namespaces

Capabilities

seccomp

AppArmor

SELinux
```

---

# Security Architecture

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

APP["Application"]

APP --> SYSCALL["System Call"]

SYSCALL --> FILTER["seccomp"]

FILTER --> ALLOW["Allow"]

FILTER --> BLOCK["Block"]
```

---

# Docker and systemd

On most Linux systems:

```text
systemd
      ↓
dockerd
      ↓
containerd
      ↓
runc
      ↓
containers
```

---

# Runtime Hierarchy

```mermaid
graph TD

SYSTEMD["systemd"]

SYSTEMD --> DOCKERD["dockerd"]

DOCKERD --> CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> PROCESS["Container Process"]
```

---

# Docker and Kubernetes

Kubernetes typically does not talk directly to Docker anymore.

Modern stack:

```text
kubelet
     ↓
containerd
     ↓
runc
```

---

# Kubernetes Runtime Architecture

```mermaid
graph TD

KUBELET["kubelet"]

KUBELET --> CONTAINERD["containerd"]

CONTAINERD --> RUNC["runc"]

RUNC --> CONTAINER["Container"]
```

---

# Observability

Important tools:

```bash
docker ps

docker inspect

docker logs

docker stats

docker events
```

---

# Debugging Container Internals

View namespaces:

```bash
lsns
```

View cgroups:

```bash
systemd-cgls
```

View processes:

```bash
ps aux
```

View networking:

```bash
ip netns

ip addr
```

---

# Common Production Problems

```text
OOM Kills

Container Restarts

Image Bloat

Network Failures

Volume Issues

PID 1 Problems

File Descriptor Exhaustion
```

---

# Production Troubleshooting Flow

```mermaid
flowchart TD

ISSUE["Container Problem"]

ISSUE --> CPU["CPU?"]

ISSUE --> MEMORY["Memory?"]

ISSUE --> NETWORK["Network?"]

ISSUE --> STORAGE["Storage?"]

CPU --> STATS["docker stats"]

MEMORY --> OOM["OOM Investigation"]

NETWORK --> IP["Network Inspection"]

STORAGE --> DF["Disk Investigation"]
```

---

# The Complete Docker Internals Map

```mermaid
mindmap
  root((Docker))

    CLI
    dockerd
    containerd
    runc

    Linux
      Namespaces
      cgroups
      OverlayFS
      Networking

    Images
      Layers
      Copy On Write

    Networking
      veth
      Bridge
      NAT

    Security
      seccomp
      Capabilities
      SELinux

    Storage
      Volumes
      Bind Mounts
```

---

# Engineering Mindset

Beginners see:

```text
docker run nginx
```

Engineers see:

```text
Docker CLI
     ↓
dockerd
     ↓
containerd
     ↓
runc
     ↓
Namespaces
     ↓
cgroups
     ↓
OverlayFS
     ↓
Linux Process
```

Docker is not magic.

Docker is Linux engineering packaged into a developer-friendly experience.

---

# Interview Questions

### What happens when you run `docker run nginx`?

### Is a container a virtual machine?

### What is containerd?

### What is runc?

### What are namespaces?

### What are cgroups?

### What is OverlayFS?

### What is Copy-On-Write?

### How does Docker networking work?

### What is a veth pair?

### What is docker0?

### Why do containers share the kernel?

### How does Docker enforce memory limits?

### What causes OOMKilled containers?

### How does Kubernetes use containerd?

---

# One-Page Architecture Summary

```text
docker run
      ↓
Docker CLI
      ↓
dockerd
      ↓
containerd
      ↓
runc
      ↓
Namespaces
      ↓
cgroups
      ↓
OverlayFS
      ↓
Linux Kernel
      ↓
Container Process
```

---

# Final Takeaway

Docker is not a virtualization technology.

Docker is an orchestration layer built on Linux kernel primitives.

Everything Docker does ultimately relies on:

```text
Namespaces

cgroups

OverlayFS

veth Networking

Capabilities

seccomp

Linux Processes
```

Master Docker internals and you gain the foundation required to understand:

```text
Containers

Kubernetes

Cloud Platforms

Platform Engineering

Modern Infrastructure
```

because beneath every container platform lies the Linux kernel.
