# Project 01: Linux System Explorer

> Your first step toward understanding how Linux sees itself.

---

# Why This Project Exists

When a Linux administrator logs into a server, one of the first questions they ask is:

> "What kind of system am I working with?"

Before installing software, debugging issues, optimizing performance, or securing a machine, engineers must understand the environment.

Consider a real-world production scenario:

A customer reports:

```text
Application is running slowly.
```

Before troubleshooting, an engineer needs answers:

* Which Linux distribution is running?
* Which kernel version is installed?
* How much memory exists?
* How many CPU cores are available?
* How much storage is available?
* What architecture is being used?
* What hostname is this machine using?
* Is this physical hardware, a VM, or a container?

Without this information, troubleshooting becomes guessing.

This project teaches how Linux exposes information about itself and how engineers collect that information systematically.

---

# Problem It Solves

Most beginners interact with Linux like this:

```text
Terminal
    ↓
Commands
    ↓
Output
```

But engineers think like this:

```text
Linux System
       ↓
Kernel
       ↓
Hardware
       ↓
Processes
       ↓
Storage
       ↓
Network
       ↓
Users
```

The goal of this project is to bridge that gap.

You will build a tool that acts as a "system investigator."

---

# Mental Model

Imagine Linux as a city.

```text
Linux City
│
├── Government      → Kernel
├── Citizens        → Users
├── Buildings       → Files
├── Roads           → Network
├── Factories       → Processes
├── Warehouses      → Storage
└── Utilities       → System Services
```

Your explorer tool is similar to a city inspector.

Its job is to gather information about every major component.

---

# Learning Objectives

By completing this project, you will understand:

* Linux distributions
* Kernel versions
* CPU architecture
* Memory information
* Storage information
* Host identity
* Linux system files
* The `/proc` filesystem
* The `/sys` filesystem
* Basic shell scripting
* Information gathering automation

---

# First Principles

Before understanding commands, we need to understand where Linux stores information.

Many beginners think:

```text
Command
     ↓
Magic Output
```

Reality:

```text
Command
     ↓
Kernel Data
     ↓
Virtual Filesystems
     ↓
Output
```

Linux exposes internal information through virtual filesystems.

The most important are:

```text
/proc
/sys
```

---

# Understanding /proc

The `/proc` directory is not stored on disk.

It is generated dynamically by the kernel.

Think of it as:

```text
Kernel Memory
       ↓
/proc
       ↓
User Access
```

Example:

```bash
cat /proc/cpuinfo
```

The CPU information does not come from a text file.

It comes directly from kernel structures.

---

# Understanding /sys

The `/sys` filesystem exposes hardware and device information.

```text
Hardware
      ↓
Kernel
      ↓
/sys
      ↓
User
```

This allows Linux to provide detailed hardware visibility.

---

# Linux Information Architecture

```mermaid
flowchart TD

A[Hardware]

A --> B[Linux Kernel]

B --> C[/proc]

B --> D[/sys]

B --> E[System Calls]

C --> F[User Commands]

D --> F

E --> F

F --> G[System Explorer]
```

---

# Project Goal

Build a command-line tool that produces a report like:

```text
=================================
LINUX SYSTEM EXPLORER
=================================

Hostname:
    prod-server-01

Operating System:
    Ubuntu 24.04

Kernel:
    6.x.x

Architecture:
    x86_64

CPU Cores:
    8

Memory:
    16 GB

Disk:
    500 GB

Uptime:
    20 Days

Load Average:
    0.75

=================================
```

---

# Project Structure

```text
linux-system-explorer/
│
├── explorer.sh
├── reports/
│   └── system-report.txt
│
└── README.md
```

---

# Step 1: Create Project Directory

```bash
mkdir linux-system-explorer

cd linux-system-explorer

mkdir reports
```

---

# Step 2: Create Script

```bash
touch explorer.sh

chmod +x explorer.sh
```

---

# Step 3: Script Header

```bash
#!/bin/bash

echo "================================="
echo "LINUX SYSTEM EXPLORER"
echo "================================="
```

---

# Exploring Host Identity

## Why It Matters

In production environments:

```text
1000 Servers
```

You must know:

```text
Which server am I connected to?
```

---

## Command

```bash
hostname
```

---

## Add To Script

```bash
echo "Hostname:"
hostname
echo
```

---

# Exploring Operating System

## Why It Matters

Different distributions behave differently.

Examples:

* Ubuntu
* Debian
* Rocky Linux
* AlmaLinux
* Fedora
* Arch Linux

Production engineers must know the OS immediately.

---

## Linux Internals

Information lives here:

```text
/etc/os-release
```

---

## Command

```bash
cat /etc/os-release
```

---

## Better Version

```bash
grep PRETTY_NAME /etc/os-release
```

---

## Add To Script

```bash
echo "Operating System:"
grep PRETTY_NAME /etc/os-release | cut -d= -f2
echo
```

---

# Exploring Kernel Version

## Why It Matters

The kernel controls:

* Processes
* Memory
* Networking
* Filesystems
* Drivers

Every system capability depends on the kernel.

---

## Command

```bash
uname -r
```

---

## Add To Script

```bash
echo "Kernel Version:"
uname -r
echo
```

---

# Exploring Architecture

## Why It Matters

Applications must match system architecture.

Examples:

```text
x86_64
arm64
aarch64
```

---

## Command

```bash
uname -m
```

---

## Add To Script

```bash
echo "Architecture:"
uname -m
echo
```

---

# Exploring CPU Information

## Why It Matters

Applications consume CPU resources.

Understanding available CPUs helps capacity planning.

---

## Linux Internals

CPU information:

```text
/proc/cpuinfo
```

---

## Commands

```bash
cat /proc/cpuinfo
```

Count CPUs:

```bash
nproc
```

---

## Add To Script

```bash
echo "CPU Cores:"
nproc
echo
```

---

# Exploring Memory

## Why It Matters

Memory exhaustion causes:

* Application crashes
* OOM kills
* Performance degradation

---

## Linux Internals

Memory statistics:

```text
/proc/meminfo
```

---

## Command

```bash
free -h
```

---

## Add To Script

```bash
echo "Memory:"
free -h
echo
```

---

# Exploring Storage

## Why It Matters

One of the most common production outages:

```text
Disk Full
```

---

## Command

```bash
df -h
```

---

## Add To Script

```bash
echo "Disk Usage:"
df -h
echo
```

---

# Exploring Uptime

## Why It Matters

Uptime indicates:

* Stability
* Recent reboots
* Maintenance history

---

## Command

```bash
uptime
```

---

## Linux Internals

Information comes from:

```text
/proc/uptime
```

---

## Add To Script

```bash
echo "System Uptime:"
uptime
echo
```

---

# Exploring Load Average

## Mental Model

Load average is a queue.

Imagine:

```text
CPU Capacity = 4 Workers

Waiting Jobs = 2

Load = 2
```

When load becomes very large:

```text
More Work
Than
CPU Capacity
```

Performance suffers.

---

## Command

```bash
uptime
```

or

```bash
cat /proc/loadavg
```

---

# Understanding Data Flow

```mermaid
flowchart LR

A[CPU]

B[Memory]

C[Storage]

D[Network]

A --> E[Kernel]

B --> E

C --> E

D --> E

E --> F[/proc]

E --> G[/sys]

F --> H[Linux Commands]

G --> H

H --> I[Explorer Script]

I --> J[System Report]
```

---

# Enhanced Version

Store report automatically.

```bash
./explorer.sh > reports/system-report.txt
```

Result:

```text
reports/system-report.txt
```

Useful for:

* Audits
* Documentation
* Troubleshooting
* Inventory collection

---

# Production Example

Imagine joining a company.

You receive SSH access:

```bash
ssh engineer@server
```

First thing many engineers do:

```bash
hostname

uname -a

free -h

df -h

uptime
```

This project automates that workflow.

---

# Docker Connection

Inside a container:

```bash
cat /etc/os-release
```

works.

But:

```bash
hostname
```

may show container identity instead of host identity.

Understanding this difference becomes important in Docker and Kubernetes.

---

# Kubernetes Connection

Kubernetes nodes use the same Linux information sources.

Examples:

```bash
kubectl debug node
```

Often leads to:

```bash
/proc
/sys
```

inspection.

Everything begins with Linux fundamentals.

---

# Security Considerations

System information can reveal:

* Hostnames
* Kernel versions
* Architecture
* Installed systems

Attackers use this information.

Be careful when sharing reports publicly.

---

# Performance Considerations

Some commands are lightweight:

```bash
hostname
uname
```

Some are more expensive:

```bash
lshw
dmidecode
```

Understanding command cost matters at scale.

---

# Troubleshooting

## Problem

Permission denied.

### Cause

Script not executable.

### Fix

```bash
chmod +x explorer.sh
```

---

## Problem

OS information missing.

### Cause

Unexpected distribution format.

### Verify

```bash
cat /etc/os-release
```

---

## Problem

CPU count incorrect.

### Verify

```bash
cat /proc/cpuinfo
```

and

```bash
nproc
```

---

# Common Mistakes

## Mistake 1

Running commands without understanding data sources.

Always ask:

```text
Where does this information come from?
```

---

## Mistake 2

Ignoring `/proc`.

Most Linux observability starts there.

---

## Mistake 3

Ignoring kernel versions.

Kernel differences matter.

---

## Mistake 4

Trusting one command blindly.

Verify using multiple sources.

---

# Engineering Mindset

A beginner sees:

```bash
free -h
```

An engineer asks:

```text
Where is memory information stored?
```

A senior engineer asks:

```text
How does the kernel calculate this?
```

A systems architect asks:

```text
How can I collect this from 10,000 servers?
```

Always move toward deeper questions.

---

# Interview Questions

### Beginner

What command displays kernel version?

### Beginner

What is hostname?

### Intermediate

What is `/proc`?

### Intermediate

Why is `/proc` called a virtual filesystem?

### Intermediate

Difference between `/proc` and `/sys`?

### Advanced

How does Linux expose CPU information?

### Advanced

How would you collect system inventory from 5000 servers?

### Advanced

How do monitoring systems gather Linux metrics?

---

# Cheat Sheet

```bash
hostname

uname -r

uname -m

cat /etc/os-release

free -h

df -h

uptime

cat /proc/cpuinfo

cat /proc/meminfo

cat /proc/loadavg

cat /proc/uptime

nproc
```

---

# Project Completion Checklist

* [ ] Created project directory
* [ ] Created explorer script
* [ ] Collected hostname
* [ ] Collected OS information
* [ ] Collected kernel information
* [ ] Collected architecture information
* [ ] Collected CPU information
* [ ] Collected memory information
* [ ] Collected storage information
* [ ] Collected uptime information
* [ ] Generated system report
* [ ] Understood `/proc`
* [ ] Understood `/sys`

---

# What You'll Understand After This Project

You will understand:

* How Linux exposes system information
* Why `/proc` exists
* Why `/sys` exists
* How administrators inspect servers
* How monitoring tools gather metrics
* How production engineers begin troubleshooting
* How Linux reveals information about itself

This is your first step from Linux user to Linux engineer.
