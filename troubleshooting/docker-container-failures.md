# Docker Container Failures Troubleshooting Guide

> One of the most important modern Linux troubleshooting skills.
>
> The root cause behind application outages, Kubernetes incidents, CI/CD failures, deployment problems, and cloud-native infrastructure issues.
>
> A topic that teaches how containers actually work beneath the Docker CLI.

---

# Why This Exists

Modern infrastructure runs on containers.

Examples:

```text
Web Applications
APIs
Databases
Microservices
CI/CD Pipelines
Kubernetes Pods
Machine Learning Workloads
```

Many engineers think:

```text
Container = Application
```

Reality:

```text
Container
=
Linux Features
+
Runtime
+
Filesystem
+
Networking
+
Process Isolation
+
Resource Control
```

When containers fail:

```text
Applications Crash
Deployments Fail
Pods Restart
Services Become Unavailable
Clusters Become Unhealthy
```

Understanding container failures means understanding Linux itself.

---

# Problem It Solves

Imagine an apartment building.

```text
Host Linux = Building

Container = Apartment
```

Each apartment has:

```text
Its Own View
Its Own Processes
Its Own Filesystem
Its Own Network
```

But all apartments still depend on:

```text
Same Building
```

When a container fails:

```text
Apartment Problem
```

When Docker fails:

```text
Building Infrastructure Problem
```

Understanding the distinction is critical.

---

# Mental Model

Most engineers troubleshoot:

```bash
docker ps
docker logs
```

and stop.

Elite engineers think:

```text
Container
 ↓
Runtime
 ↓
Namespaces
 ↓
Cgroups
 ↓
Filesystem
 ↓
Network
 ↓
Kernel
```

Every container issue occurs somewhere in this stack.

---

# First Principles

A container is NOT:

```text
A Virtual Machine
```

Containers share:

```text
Host Kernel
```

Unlike VMs:

```mermaid
flowchart TD

A[Application]

B[Container]

C[Host Kernel]

D[Hardware]

A --> B
B --> C
C --> D
```

There is:

```text
One Kernel
```

for all containers.

---

# Docker Architecture

```mermaid
flowchart TD

A[Docker CLI]

B[Docker Daemon]

C[containerd]

D[runc]

E[Namespaces]

F[Cgroups]

G[OverlayFS]

H[Linux Kernel]

A --> B
B --> C
C --> D

D --> E
D --> F
D --> G

E --> H
F --> H
G --> H
```

Failure can occur at any layer.

---

# The Golden Rule

Never ask:

```text
Why Did The Container Fail?
```

Ask:

```text
Which Layer Failed?
```

Possible layers:

```text
Application
Process
Filesystem
Network
Resources
Container Runtime
Docker Engine
Kernel
```

---

# Container Lifecycle

```mermaid
flowchart LR

A[Image]

B[Container Created]

C[Container Started]

D[Running]

E[Stopped]

F[Removed]

A --> B
B --> C
C --> D
D --> E
E --> F
```

Understanding where failure occurs dramatically speeds troubleshooting.

---

# Common Failure Categories

```text
Container Won't Start

Container Exits Immediately

CrashLoop

OOMKilled

Network Failures

Storage Failures

Permission Issues

Docker Daemon Failures

Image Pull Failures

Runtime Failures
```

---

# Failure 1: Container Exits Immediately

Most common issue.

Example:

```bash
docker run nginx
```

Container exits instantly.

---

# Why?

Containers live as long as:

```text
PID 1 Lives
```

When PID 1 exits:

```text
Container Stops
```

---

# Process Architecture

```mermaid
flowchart TD

A[Container]

B[PID 1]

C[Worker Processes]

A --> B
B --> C
```

No PID 1:

```text
No Container
```

---

# Investigation

Check:

```bash
docker logs CONTAINER
```

Check:

```bash
docker inspect CONTAINER
```

---

# Failure 2: CrashLoop

Container repeatedly:

```text
Start
Crash
Restart
Crash
Restart
```

---

# CrashLoop Flow

```mermaid
flowchart TD

A[Container Start]

B[Application Crash]

C[Restart Policy]

D[Restart]

A --> B
B --> C
C --> D
D --> B
```

---

# Common Causes

```text
Bad Configuration
Missing Environment Variables
Database Unreachable
Application Bugs
Permission Errors
```

---

# Investigation

```bash
docker logs CONTAINER
```

Most useful command.

---

# Failure 3: OOMKilled

Very common in production.

Container disappears.

Exit code:

```text
137
```

---

# Why?

Linux memory exhausted.

Kernel activates:

```text
OOM Killer
```

---

# Memory Pressure Architecture

```mermaid
flowchart TD

A[Container]

B[Cgroup Memory Limit]

C[Memory Exhaustion]

D[OOM Killer]

E[Container Killed]

A --> B
B --> C
C --> D
D --> E
```

---

# Investigation

Check:

```bash
docker inspect CONTAINER
```

Look for:

```text
OOMKilled=true
```

Check:

```bash
dmesg
```

---

# Failure 4: Image Pull Failure

Example:

```text
ImagePullBackOff
```

or

```text
pull access denied
```

---

# Causes

```text
Wrong Image Name
Private Registry Authentication
DNS Failure
Network Failure
Registry Outage
```

---

# Image Pull Architecture

```mermaid
flowchart LR

A[Docker Client]

B[Registry]

C[Image Layers]

D[Host]

A --> B
B --> C
C --> D
```

---

# Investigation

```bash
docker pull IMAGE
```

Verify:

```bash
ping registry
```

---

# Failure 5: Container Networking Failure

Example:

```text
Container Running

Application Unreachable
```

---

# Networking Stack

```mermaid
flowchart LR

A[Container]

B[veth]

C[Docker Bridge]

D[Host Network]

E[Internet]

A --> B
B --> C
C --> D
D --> E
```

---

# Common Causes

```text
Port Mapping Missing
Bridge Issues
Firewall Rules
DNS Failure
Application Not Listening
```

---

# Investigation

Check:

```bash
docker ps
```

Look for:

```text
0.0.0.0:8080->80
```

Verify listening:

```bash
docker exec CONTAINER ss -tulpn
```

---

# Failure 6: DNS Resolution Failure

Inside container:

```bash
curl google.com
```

fails.

---

# Docker DNS Flow

```mermaid
flowchart TD

A[Container]

B[Docker DNS]

C[Host Resolver]

D[DNS Server]

A --> B
B --> C
C --> D
```

---

# Investigation

```bash
cat /etc/resolv.conf
```

inside container.

---

# Failure 7: Volume Mount Problems

Example:

```text
File Missing
Data Lost
Configuration Missing
```

---

# Storage Architecture

```mermaid
flowchart TD

A[Container]

B[Volume]

C[Host Filesystem]

D[Disk]

A --> B
B --> C
C --> D
```

---

# Investigation

Check:

```bash
docker inspect CONTAINER
```

Look for:

```text
Mounts
```

section.

---

# Failure 8: OverlayFS Issues

Containers use:

```text
OverlayFS
```

---

# OverlayFS Architecture

```mermaid
flowchart TD

A[Lower Image Layer]

B[Upper Writable Layer]

C[Merged View]

A --> C
B --> C
```

Problems:

```text
Disk Full
Corruption
Overlay Mount Failure
```

---

# Investigation

Check:

```bash
df -h
```

and

```bash
docker system df
```

---

# Failure 9: Docker Daemon Failure

Example:

```bash
docker ps
```

returns:

```text
Cannot connect to Docker daemon
```

---

# Docker Runtime Stack

```mermaid
flowchart TD

A[Docker CLI]

B[Docker Daemon]

C[containerd]

D[runc]

E[Kernel]

A --> B
B --> C
C --> D
D --> E
```

---

# Investigation

Check:

```bash
systemctl status docker
```

Check:

```bash
journalctl -u docker
```

---

# Failure 10: Permission Denied

Example:

```text
permission denied
```

---

# Common Causes

```text
SELinux
AppArmor
Volume Permissions
UID/GID Mismatch
Read-Only Filesystem
```

---

# User Mapping Problem

```mermaid
flowchart TD

A[Container User]

B[Host File]

C[UID Match?]

D[Access Granted]

E[Permission Denied]

A --> C
B --> C

C --> D
C --> E
```

---

# Linux Internals

Containers rely heavily on:

```text
Namespaces
Cgroups
OverlayFS
Capabilities
Seccomp
```

---

# Namespace Isolation

```mermaid
flowchart TD

A[Container A]

B[Container B]

C[PID Namespace]

D[Network Namespace]

E[Mount Namespace]

A --> C
A --> D
A --> E

B --> C
B --> D
B --> E
```

---

# Resource Isolation

```mermaid
flowchart TD

A[Cgroups]

B[CPU Limits]

C[Memory Limits]

D[IO Limits]

A --> B
A --> C
A --> D
```

---

# Production Incident Example

## Incident

E-commerce API outage.

Monitoring:

```text
Containers Restarting
```

Investigation:

```bash
docker ps -a
```

Found:

```text
Exited (137)
```

Check:

```bash
docker inspect
```

Result:

```text
OOMKilled=true
```

Root Cause:

```text
Memory Leak
```

Fix:

```text
Application Patch
Increase Memory Limit
```

---

# Kubernetes Connection

Every Kubernetes pod is:

```text
A Container
```

Most pod failures are actually:

```text
Container Failures
```

Examples:

```text
CrashLoopBackOff
OOMKilled
ImagePullBackOff
ContainerCreating
```

Understanding Docker troubleshooting directly improves Kubernetes troubleshooting.

---

# CI/CD Connection

Pipeline failures often originate from:

```text
Container Build Errors
Registry Authentication
Image Pull Problems
```

not CI systems themselves.

---

# Cloud Connection

Cloud-native systems depend heavily on:

```text
Containers
```

Failures impact:

```text
ECS
EKS
AKS
GKE
Nomad
OpenShift
```

---

# Performance Considerations

Monitor:

```text
CPU Usage
Memory Usage
Disk IO
Network Throughput
```

Commands:

```bash
docker stats
```

```bash
cgroup metrics
```

---

# Security Considerations

Common issues:

```text
Privileged Containers
Root Containers
Exposed Docker Socket
Excessive Capabilities
```

Always investigate:

```text
Who Can Access What
```

inside container.

---

# Observability

Essential commands:

```bash
docker ps

docker logs

docker inspect

docker stats

docker exec

docker events

journalctl -u docker
```

---

# Master Troubleshooting Workflow

```mermaid
flowchart TD

A[Container Failure]

B[Running?]

C[Logs]

D[Resources]

E[Networking]

F[Storage]

G[Runtime]

H[Kernel]

I[Root Cause]

A --> B
B --> C
C --> D
D --> E
E --> F
F --> G
G --> H
H --> I
```

---

# Common Mistakes

## Mistake 1

Treating containers as virtual machines.

---

## Mistake 2

Ignoring container logs.

---

## Mistake 3

Ignoring PID 1 behavior.

---

## Mistake 4

Blaming Docker before checking application.

---

## Mistake 5

Ignoring resource limits.

---

## Mistake 6

Ignoring host-level failures.

---

# Engineering Mindset

Beginners think:

```text
Docker Is Broken
```

Engineers think:

```text
Container Failed
```

Senior engineers think:

```text
Which Layer Failed?
```

Elite platform engineers think:

```text
Application
↓
Container
↓
Runtime
↓
Namespaces
↓
Cgroups
↓
Kernel

Where Did The Failure Actually Occur?
```

Because containers are not magic.

They are Linux features assembled into a platform.

---

# Interview Questions

### Why does a container stop?

Because PID 1 exited.

---

### What does Exit Code 137 mean?

Usually OOMKilled.

---

### Difference between image and container?

Image:

```text
Template
```

Container:

```text
Running Instance
```

---

### What command shows container logs?

```bash
docker logs CONTAINER
```

---

### What command shows resource usage?

```bash
docker stats
```

---

### What Linux features power containers?

```text
Namespaces
Cgroups
OverlayFS
Capabilities
```

---

### Can a container have its own kernel?

No.

Containers share host kernel.

---

# Cheat Sheet

```bash
# Running Containers
docker ps

# All Containers
docker ps -a

# Logs
docker logs CONTAINER

# Detailed Info
docker inspect CONTAINER

# Resource Usage
docker stats

# Execute Shell
docker exec -it CONTAINER sh

# Networks
docker network ls

# Volumes
docker volume ls

# Images
docker images

# Pull Image
docker pull IMAGE

# Events
docker events

# Docker Logs
journalctl -u docker
```

---

# Final Takeaway

Container failures are rarely:

```text
Docker Problems
```

Most are:

```text
Application Problems
Resource Problems
Filesystem Problems
Network Problems
Kernel Problems
```

The most important lesson:

```text
Container
≠
Magic Box
```

A container is simply:

```text
Linux Namespaces
+
Cgroups
+
Filesystem Layers
+
Networking
+
Processes
```

The best Linux engineers troubleshoot containers by peeling back layers:

```text
Application
 ↓
Container
 ↓
Runtime
 ↓
Kernel
```

until they discover the real root cause.

That mindset scales from:

```text
Single Docker Container
        ↓
Docker Host
        ↓
Kubernetes Pod
        ↓
Production Cluster
        ↓
Cloud Platform
```

and is one of the defining skills of modern Linux, DevOps, SRE, and Platform Engineers.
