# Linux Namespaces & Cgroups — The Foundation of Docker, Kubernetes, and Modern Linux

> If you understand:
>
> ```text
> Processes
> Threads
> fork()
> Signals
> PID 1
> ```
>
> then the next big question is:
>
> ```text
> How can Linux make one process think
> it has its own machine?
> ```
>
> The answer is:
>
> ```text
> Namespaces
> +
> Cgroups
> ```
>
> These two kernel technologies are the foundation of:
>
> - Docker
> - Kubernetes
> - Podman
> - containerd
> - LXC
> - OpenShift
> - Cloud Platforms
>
> Understanding them is essential for modern Linux.

---

# Learning Objectives

After this guide, you should understand:

```text
✓ What namespaces are
✓ What cgroups are
✓ Process isolation
✓ Resource control
✓ Container internals
✓ Docker architecture
✓ Kubernetes architecture
✓ PID namespaces
✓ Network namespaces
✓ Mount namespaces
✓ UTS namespaces
✓ IPC namespaces
✓ User namespaces
✓ Cgroup v1 vs v2
✓ CPU limits
✓ Memory limits
✓ Modern Linux containers
```

---

# The Big Problem

Imagine:

```text
Server

 ├── Application A
 ├── Application B
 ├── Application C
```

All share:

```text
Processes
Network
Filesystem
Memory
Hostnames
Users
```

This creates problems.

---

# Example

Application A sees:

```bash
ps aux
```

Output:

```text
ALL SYSTEM PROCESSES
```

Not ideal.

---

# Another Problem

Application A can see:

```text
Application B Files
```

Potential security issue.

---

# Another Problem

Application A uses:

```text
100% CPU
```

Application B becomes slow.

---

# Linux Needed Two Things

---

## Isolation

```text
Hide Resources
```

---

## Resource Control

```text
Limit Resource Usage
```

---

# Linux Solution

```text
Namespaces

+

Cgroups
```

Visual:

```text
Namespaces
     │
Isolation

Cgroups
     │
Control
```

---

# Child-Friendly Explanation

Imagine a school.

Students share:

```text
Classroom
Playground
Library
```

Teacher creates:

```text
Room A

Room B

Room C
```

Each group believes:

```text
This Is Our World
```

That's a namespace.

---

# What is a Namespace?

A namespace provides:

```text
Isolation
```

for a particular system resource.

Visual:

```text
Host System

      │

Namespaces

      │

 ┌────┼────┐

 ▼    ▼    ▼

AppA AppB AppC
```

---

# Simple Definition

Namespace:

```text
Changes what a process can see.
```

---

# Example

Without namespace:

```bash
ps aux
```

shows:

```text
All Processes
```

With namespace:

```bash
ps aux
```

shows:

```text
Only Container Processes
```

---

# What is a Cgroup?

Cgroup means:

```text
Control Group
```

Provides:

```text
Resource Management
```

---

# Simple Definition

Cgroup:

```text
Controls how much a process can use.
```

---

# Visual

```text
Namespace
     │
What You Can See

Cgroup
     │
What You Can Use
```

---

# The Container Formula

Modern containers are basically:

```text
Namespaces

+

Cgroups

+

Filesystem

+

Process
```

---

# Visual

```text
Container

 ├── Namespaces
 ├── Cgroups
 ├── Root Filesystem
 └── Application
```

---

# Linux Namespace Types

Linux supports multiple namespaces.

---

# Overview

```text
PID

Network

Mount

UTS

IPC

User

Cgroup

Time
```

---

# Visual

```text
Linux Namespaces

 ├── PID
 ├── NET
 ├── MNT
 ├── UTS
 ├── IPC
 ├── USER
 ├── CGROUP
 └── TIME
```

---

# PID Namespace

One of the most important namespaces.

Controls:

```text
Process Visibility
```

---

# Without PID Namespace

```bash
ps aux
```

Output:

```text
Every Process
```

---

# With PID Namespace

```bash
ps aux
```

Output:

```text
Only Container Processes
```

---

# Visual

Host:

```text
PID 1

PID 100

PID 200

PID 300
```

Container:

```text
PID 1

PID 2

PID 3
```

Same processes.

Different view.

---

# Why PID Namespace Matters

Container believes:

```text
I Have My Own PID 1
```

Reality:

```text
Host Has Different PID
```

---

# Docker Example

Inside container:

```bash
ps
```

Output:

```text
PID 1 app.py
```

Inside container:

```text
app.py thinks it is PID 1
```

---

# Network Namespace

Provides:

```text
Separate Network Stack
```

---

# Each Namespace Gets

```text
Interfaces

Routing Tables

ARP Cache

Firewall Rules

Ports
```

---

# Visual

```text
Container A

eth0
10.0.0.2


Container B

eth0
10.0.0.3
```

Each sees its own network.

---

# Why Important?

Both containers can run:

```text
Port 80
```

without conflict.

---

# Mount Namespace

Provides:

```text
Filesystem Isolation
```

---

# Example

Container sees:

```text
/app

/bin

/etc
```

Host sees:

```text
/
```

Different filesystem views.

---

# Visual

```text
Host FS

      │

Mount Namespace

      │

Container View
```

---

# UTS Namespace

Controls:

```text
Hostname

Domain Name
```

---

# Example

Host:

```text
server01
```

Container:

```text
nginx-container
```

Both coexist.

---

# IPC Namespace

IPC means:

```text
Inter Process Communication
```

Controls:

```text
Shared Memory

Message Queues

Semaphores
```

---

# Why?

Container A should not access:

```text
Container B Shared Memory
```

---

# User Namespace

Provides:

```text
User Isolation
```

---

# Powerful Feature

Inside container:

```text
root
```

Outside container:

```text
regular user
```

---

# Visual

Container:

```text
UID 0
```

Host:

```text
UID 1000
```

Mapped safely.

---

# Security Benefit

Container root:

```text
Not Real Host Root
```

Huge security improvement.

---

# Cgroup Namespace

Provides isolated view of:

```text
Control Groups
```

---

# Time Namespace

Newer namespace.

Provides:

```text
Independent Time View
```

Useful for:

```text
Testing

Checkpointing

Containers
```

---

# Viewing Namespaces

Command:

```bash
lsns
```

Example:

```text
NS TYPE

4026531836 pid

4026531993 net
```

---

# Create Namespace

Example:

```bash
unshare --pid --fork bash
```

Creates:

```text
New PID Namespace
```

---

# Enter Existing Namespace

Example:

```bash
nsenter
```

---

# Visual

```text
Existing Namespace

      │

nsenter

      ▼

Join Namespace
```

---

# What are Cgroups?

Namespaces isolate.

Cgroups control.

---

# Example Problem

Application:

```text
Uses 100% CPU

Consumes 50GB RAM
```

Need limits.

---

# Cgroup Solution

```text
CPU Limit

Memory Limit

I/O Limit
```

---

# Visual

```text
Application

      │

Cgroup

      │

Resource Limits
```

---

# CPU Limits

Example:

```text
Max 2 CPUs
```

Even on:

```text
64 Core Server
```

---

# Memory Limits

Example:

```text
512MB RAM
```

Process cannot exceed limit.

---

# Visual

```text
Memory

512MB Max

██████████
```

---

# If Limit Exceeded?

Kernel may trigger:

```text
OOM Killer
```

(Out Of Memory Killer)

---

# OOM Killer

Purpose:

```text
Protect System
```

Visual:

```text
Memory Exhausted
       │
       ▼
OOM Killer
       │
       ▼
Process Terminated
```

---

# I/O Limits

Control:

```text
Disk Bandwidth

Disk Operations
```

Useful for:

```text
Multi-Tenant Systems
```

---

# Network Limits

Often implemented through:

```text
Traffic Control

Cgroups

Container Runtime
```

---

# Cgroup v1 vs v2

Modern Linux uses:

```text
Cgroup v2
```

---

# Cgroup v1

Older design.

Multiple hierarchies.

Complex.

---

# Cgroup v2

Modern design.

Unified hierarchy.

Simpler.

Better resource management.

---

# Visual

v1:

```text
CPU Tree

Memory Tree

I/O Tree
```

---

v2:

```text
Single Unified Tree
```

---

# View Cgroups

```bash
cat /proc/self/cgroup
```

---

# Modern Linux

Check:

```bash
mount | grep cgroup
```

Usually:

```text
cgroup2
```

---

# systemd and Cgroups

Modern Linux services are organized using:

```text
systemd

+

cgroups
```

---

# Visual

```text
systemd

 ├── nginx.service
 ├── ssh.service
 └── docker.service
```

Each gets its own cgroup.

---

# Docker Internals

Docker uses:

```text
Namespaces

Cgroups

OverlayFS
```

---

# Visual

```text
Docker Container

 ├── PID Namespace
 ├── NET Namespace
 ├── MNT Namespace
 ├── User Namespace
 ├── Cgroup
 └── App
```

---

# Kubernetes Internals

Kubernetes does NOT create isolation.

Linux does.

Kubernetes orchestrates containers.

Linux provides:

```text
Namespaces

Cgroups
```

---

# Visual

```text
Kubernetes

      │

Container Runtime

      │

Linux Kernel

      │

Namespaces + Cgroups
```

---

# Real Production Example

Node:

```text
100 Containers
```

How isolated?

```text
Namespaces
```

How limited?

```text
Cgroups
```

---

# Common Beginner Mistakes

---

## ❌ Containers Are Virtual Machines

Wrong.

Containers share kernel.

---

## ❌ Docker Creates Isolation

Partly.

Linux namespaces create isolation.

---

## ❌ Cgroups Create Isolation

Wrong.

Namespaces isolate.

Cgroups limit.

---

## ❌ Container Root = Host Root

Wrong with user namespaces.

---

## ❌ Kubernetes Replaces Linux

Wrong.

Kubernetes relies on Linux features.

---

# Interview Questions

## What provides process isolation?

```text
PID Namespace
```

---

## What provides network isolation?

```text
Network Namespace
```

---

## What controls resource usage?

```text
Cgroups
```

---

## Difference Between Namespace and Cgroup?

Namespace:

```text
Isolation
```

Cgroup:

```text
Resource Control
```

---

## What is PID 1 inside a container?

Container's init process.

---

## What is Cgroup v2?

Modern unified cgroup hierarchy.

---

## How does Docker isolate containers?

Using Linux namespaces and cgroups.

---

## Does Kubernetes create namespaces?

Linux kernel creates namespaces.

Kubernetes orchestrates usage.

---

# Visual Summary

```text
Namespaces
      │
Isolation
```

```text
Cgroups
      │
Control
```

Container Formula:

```text
Container

=

Namespaces

+

Cgroups

+

Filesystem

+

Process
```

Namespaces:

```text
PID

NET

MNT

UTS

IPC

USER

CGROUP

TIME
```

Resource Control:

```text
CPU

Memory

I/O

Network
```

---

# Final Takeaway

Modern containers are not magic.

At their core they are simply:

```text
Linux Processes
```

with:

```text
Namespaces
```

providing:

```text
Isolation
```

and:

```text
Cgroups
```

providing:

```text
Resource Limits
```

These two technologies are the foundation of:

```text
Docker

Kubernetes

Podman

containerd

OpenShift

Cloud Native Infrastructure
```

Mastering Namespaces and Cgroups is one of the most important steps in understanding how modern Linux, containers, and cloud platforms actually work under the hood.
