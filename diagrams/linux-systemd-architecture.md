# Linux systemd Architecture

## The Complete Guide to PID 1, Services, Targets, Dependencies, Logging, Timers, and Modern Linux Initialization

---

# Why This Exists

When a Linux system boots, something must answer:

```text
What starts first?

What starts next?

What happens if a service fails?

Who manages services?

Who handles logging?

Who mounts filesystems?

Who starts networking?

Who restarts crashed applications?
```

Historically Linux used:

```text
SysVinit
Upstart
```

Modern Linux uses:

```text
systemd
```

Today systemd is the operating system manager for most Linux distributions.

Examples:

```text
Ubuntu
Debian
RHEL
Rocky Linux
AlmaLinux
Fedora
SUSE
Arch Linux
```

Understanding systemd is essential for:

* Linux Engineers
* DevOps Engineers
* SREs
* Platform Engineers
* Cloud Engineers
* Infrastructure Architects

---

# The systemd Mental Model

Most beginners think:

```text
systemd = Service Manager
```

Reality:

```text
systemd = Operating System Manager
```

systemd controls:

```text
Boot Process

Services

Logging

Timers

Networking

Mounts

Sockets

Resource Management

Dependency Resolution
```

Think of systemd as:

```text
The CEO of Userspace
```

The Linux kernel manages hardware.

systemd manages the operating system.

---

# The Big Picture

```mermaid
graph TD

BOOT["Kernel Boot Complete"]

BOOT --> PID1["systemd (PID 1)"]

PID1 --> SERVICES["Services"]

PID1 --> NETWORK["Networking"]

PID1 --> MOUNTS["Mounts"]

PID1 --> LOGGING["Logging"]

PID1 --> TIMERS["Timers"]

PID1 --> USERS["User Sessions"]
```

---

# Linux Boot Architecture

Understanding systemd begins with booting.

```mermaid
flowchart TD

POWER["Power On"]

POWER --> FIRMWARE["BIOS / UEFI"]

FIRMWARE --> GRUB["GRUB"]

GRUB --> KERNEL["Linux Kernel"]

KERNEL --> INITRAMFS["Initramfs"]

INITRAMFS --> SYSTEMD["systemd PID 1"]

SYSTEMD --> SERVICES["System Services"]

SERVICES --> USERS["Users"]
```

---

# Why PID 1 Matters

The first userspace process is:

```text
PID 1
```

On modern Linux:

```bash
ps -p 1
```

Output:

```text
systemd
```

---

# PID Hierarchy

```mermaid
graph TD

SYSTEMD["systemd (PID 1)"]

SYSTEMD --> SSHD["sshd"]

SYSTEMD --> NGINX["nginx"]

SYSTEMD --> POSTGRES["postgresql"]

SYSTEMD --> DOCKER["docker"]

SYSTEMD --> KUBELET["kubelet"]

SSHD --> BASH["bash"]

BASH --> VIM["vim"]
```

Every process ultimately traces back to PID 1.

---

# Responsibilities of PID 1

systemd is responsible for:

```text
Service Startup

Dependency Resolution

Process Supervision

Zombie Reaping

Logging

Shutdown

Reboot

Mount Management
```

---

# systemd Architecture Overview

```mermaid
graph TD

SYSTEMD["systemd"]

SYSTEMD --> UNIT["Unit Manager"]

SYSTEMD --> JOURNAL["journald"]

SYSTEMD --> LOGIND["logind"]

SYSTEMD --> NETWORKD["networkd"]

SYSTEMD --> RESOLVED["resolved"]

SYSTEMD --> TIMERS["Timers"]

SYSTEMD --> MOUNTS["Mounts"]

SYSTEMD --> SOCKETS["Sockets"]
```

---

# What Is a Unit?

Everything in systemd is represented as a unit.

Think:

```text
Unit = Managed Object
```

Examples:

```text
Service

Timer

Socket

Mount

Target

Device

Path
```

---

# Unit Architecture

```mermaid
graph TD

UNIT["Unit"]

UNIT --> SERVICE[".service"]

UNIT --> SOCKET[".socket"]

UNIT --> TIMER[".timer"]

UNIT --> TARGET[".target"]

UNIT --> MOUNT[".mount"]

UNIT --> PATH[".path"]
```

---

# Common Unit Types

| Unit     | Purpose           |
| -------- | ----------------- |
| .service | Service           |
| .socket  | Socket Activation |
| .target  | System State      |
| .mount   | Filesystem Mount  |
| .timer   | Scheduled Task    |
| .path    | File Monitoring   |
| .device  | Device            |
| .slice   | Resource Group    |

---

# Service Units

Most engineers interact primarily with services.

Example:

```text
nginx.service

sshd.service

docker.service

postgresql.service
```

---

# Service Architecture

```mermaid
graph TD

SERVICE["nginx.service"]

SERVICE --> EXEC["ExecStart"]

SERVICE --> RESTART["Restart Policy"]

SERVICE --> DEPENDS["Dependencies"]

SERVICE --> LOGS["Logs"]
```

---

# Service Lifecycle

```mermaid
stateDiagram-v2

STOPPED --> STARTING

STARTING --> RUNNING

RUNNING --> RELOADING

RELOADING --> RUNNING

RUNNING --> STOPPING

STOPPING --> STOPPED

RUNNING --> FAILED
```

---

# Service Management Commands

Start:

```bash
systemctl start nginx
```

Stop:

```bash
systemctl stop nginx
```

Restart:

```bash
systemctl restart nginx
```

Reload:

```bash
systemctl reload nginx
```

Status:

```bash
systemctl status nginx
```

---

# Unit File Structure

Typical file:

```bash
/usr/lib/systemd/system/nginx.service
```

or

```bash
/etc/systemd/system/
```

---

# Unit File Anatomy

```ini
[Unit]
Description=Nginx Web Server

[Service]
ExecStart=/usr/sbin/nginx

[Install]
WantedBy=multi-user.target
```

---

# Unit Sections

```mermaid
graph TD

UNITFILE["Unit File"]

UNITFILE --> UNIT["[Unit]"]

UNITFILE --> SERVICE["[Service]"]

UNITFILE --> INSTALL["[Install]"]
```

---

# Dependency Management

One of systemd's biggest strengths.

---

# Dependency Graph

```mermaid
graph TD

NETWORK["network.target"]

NETWORK --> POSTGRES["postgresql.service"]

POSTGRES --> APP["application.service"]

APP --> NGINX["nginx.service"]
```

---

# Dependency Types

Common relationships:

```text
Requires=

Wants=

Before=

After=

Conflicts=
```

---

# Boot Dependency Resolution

```mermaid
flowchart TD

SYSTEMD["systemd"]

SYSTEMD --> NETWORK["Network"]

NETWORK --> DATABASE["Database"]

DATABASE --> APP["Application"]

APP --> LOADBALANCER["Load Balancer"]
```

---

# Targets

Targets represent system states.

Equivalent to runlevels.

---

# Target Hierarchy

```mermaid
graph TD

BASIC["basic.target"]

BASIC --> MULTI["multi-user.target"]

MULTI --> GRAPHICAL["graphical.target"]
```

---

# Common Targets

| Target            | Purpose         |
| ----------------- | --------------- |
| rescue.target     | Recovery        |
| multi-user.target | Server          |
| graphical.target  | Desktop         |
| emergency.target  | Emergency Shell |

---

# View Current Target

```bash
systemctl get-default
```

---

# Change Target

```bash
systemctl set-default multi-user.target
```

---

# Socket Activation

One of systemd's most powerful features.

---

# Traditional Startup

```mermaid
flowchart LR

BOOT["Boot"]

BOOT --> SERVICE["Start Service"]

SERVICE --> WAIT["Wait For Requests"]
```

Resources wasted.

---

# Socket Activation

```mermaid
flowchart LR

BOOT["Boot"]

BOOT --> SOCKET["Open Socket"]

SOCKET --> REQUEST["Incoming Request"]

REQUEST --> SERVICE["Start Service"]
```

Start only when needed.

---

# Socket Architecture

```mermaid
graph TD

CLIENT["Client"]

CLIENT --> SOCKET["Socket Unit"]

SOCKET --> SERVICE["Service Unit"]
```

---

# Mount Management

systemd manages filesystems.

---

# Mount Architecture

```mermaid
graph TD

SYSTEMD["systemd"]

SYSTEMD --> MOUNT["Mount Unit"]

MOUNT --> FILESYSTEM["Filesystem"]

FILESYSTEM --> STORAGE["Storage Device"]
```

---

# View Mounts

```bash
systemctl list-units --type=mount
```

---

# Logging Architecture

Modern Linux logging revolves around journald.

---

# Logging Flow

```mermaid
flowchart TD

APPLICATION["Application"]

APPLICATION --> JOURNALD["journald"]

JOURNALD --> STORAGE["Journal Storage"]

STORAGE --> JOURNALCTL["journalctl"]
```

---

# journald Architecture

```mermaid
graph TD

SERVICES["Services"]

SERVICES --> JOURNALD["journald"]

JOURNALD --> MEMORY["Memory"]

JOURNALD --> DISK["Persistent Storage"]
```

---

# View Logs

Entire system:

```bash
journalctl
```

Current boot:

```bash
journalctl -b
```

Specific service:

```bash
journalctl -u nginx
```

Live logs:

```bash
journalctl -f
```

---

# Logging Pipeline

```mermaid
flowchart LR

SERVICE["Service"]

SERVICE --> STDOUT["stdout"]

STDOUT --> JOURNALD["journald"]

JOURNALD --> QUERY["journalctl"]
```

---

# Timers

systemd timers replace cron in many environments.

---

# Timer Architecture

```mermaid
graph TD

TIMER["Backup.timer"]

TIMER --> SERVICE["Backup.service"]

SERVICE --> TASK["Backup Job"]
```

---

# Timer Flow

```mermaid
flowchart LR

TIME["Scheduled Time"]

TIME --> TIMER["Timer Unit"]

TIMER --> SERVICE["Service Unit"]

SERVICE --> EXECUTE["Task Runs"]
```

---

# View Timers

```bash
systemctl list-timers
```

---

# Resource Management

systemd integrates with cgroups.

---

# Resource Architecture

```mermaid
graph TD

SERVICE["Service"]

SERVICE --> CGROUP["cgroup"]

CGROUP --> CPU["CPU Limit"]

CGROUP --> MEMORY["Memory Limit"]

CGROUP --> IO["I/O Limit"]
```

---

# Example Limits

```ini
[Service]
MemoryMax=1G
CPUQuota=50%
```

---

# Failure Recovery

One of systemd's strongest capabilities.

---

# Recovery Architecture

```mermaid
flowchart TD

SERVICE["Service"]

SERVICE --> CRASH["Crash"]

CRASH --> SYSTEMD["systemd"]

SYSTEMD --> RESTART["Restart"]

RESTART --> RUNNING["Running"]
```

---

# Automatic Restart

```ini
[Service]
Restart=always
RestartSec=5
```

---

# Production Service Supervision

```mermaid
graph TD

NGINX["Nginx"]

NGINX --> SYSTEMD["systemd"]

SYSTEMD --> MONITOR["Monitor"]

MONITOR --> RESTART["Restart If Needed"]
```

---

# User Sessions

Managed by:

```text
systemd-logind
```

---

# Session Architecture

```mermaid
graph TD

USER["User"]

USER --> LOGIN["Login"]

LOGIN --> LOGIND["systemd-logind"]

LOGIND --> SESSION["Session"]
```

---

# systemd and Containers

Many containers avoid running full systemd.

Reason:

```text
Containers share host kernel
```

However:

```text
Podman
System Containers
LXC
```

may use systemd internally.

---

# Container Architecture

```mermaid
graph TD

HOST["Host systemd"]

HOST --> CONTAINER["Container"]

CONTAINER --> PROCESS["Application"]
```

---

# systemd and Kubernetes

Kubernetes nodes rely heavily on systemd.

---

# Node Architecture

```mermaid
graph TD

SYSTEMD["systemd"]

SYSTEMD --> KUBELET["kubelet"]

SYSTEMD --> CONTAINERD["containerd"]

KUBELET --> PODS["Pods"]
```

---

# Boot Performance Analysis

Essential tool:

```bash
systemd-analyze
```

---

# Boot Performance Flow

```mermaid
graph TD

BOOT["Boot"]

BOOT --> FIRMWARE["Firmware"]

BOOT --> KERNEL["Kernel"]

BOOT --> INITRAMFS["Initramfs"]

BOOT --> SYSTEMD["systemd"]

SYSTEMD --> SERVICES["Services"]
```

---

# Slow Service Investigation

```bash
systemd-analyze blame
```

---

# Critical Dependency Chain

```bash
systemd-analyze critical-chain
```

---

# Troubleshooting Workflow

```mermaid
flowchart TD

ISSUE["Service Problem"]

ISSUE --> STATUS["systemctl status"]

STATUS --> LOGS["journalctl"]

LOGS --> FAILED["Failed?"]

FAILED --> RESTART["Restart"]

RESTART --> VERIFY["Verify"]
```

---

# Production Failure Scenarios

## Nginx Not Starting

```bash
systemctl status nginx
journalctl -u nginx
```

---

## Failed Boot

```bash
systemctl --failed
```

---

## Slow Boot

```bash
systemd-analyze blame
```

---

## Dependency Failure

```bash
systemctl list-dependencies
```

---

# Common Mistakes

### Using SIGKILL Immediately

Prefer:

```bash
systemctl stop service
```

---

### Ignoring Dependencies

A service may fail because another service failed.

---

### Ignoring Journald

Most answers already exist in logs.

---

### Running Processes Outside systemd

Loses supervision and recovery.

---

### Not Configuring Restart Policies

Causes avoidable downtime.

---

# Production Engineering Mindset

Beginners see:

```text
systemctl start nginx
```

Engineers see:

```text
Unit Files
     ↓
Dependency Graph
     ↓
Targets
     ↓
cgroups
     ↓
Logging
     ↓
Recovery
     ↓
Observability
```

systemd is not merely a service manager.

It is the operating system control plane.

---

# Interview Questions

### What is systemd?

### Why is PID 1 special?

### What is a unit?

### Difference between service and target?

### What is journald?

### What is socket activation?

### What are systemd timers?

### What is a dependency graph?

### What is multi-user.target?

### How do you troubleshoot failed services?

### How does systemd integrate with cgroups?

### How does systemd handle automatic recovery?

### How does systemd improve boot performance?

### What replaced cron in systemd?

### How does Kubernetes use systemd?

---

# One-Page Architecture Summary

```text
Linux Boot
      ↓
Kernel
      ↓
systemd (PID 1)
      ↓
Targets
      ↓
Services
      ↓
Logging
      ↓
Timers
      ↓
Resource Management
      ↓
Application Availability
```

---

# Final Takeaway

systemd is the central control plane of modern Linux.

It manages:

```text
Boot Process

Services

Dependencies

Targets

Logging

Timers

Mounts

Sockets

Resource Controls

Failure Recovery
```

Every modern Linux server, cloud VM, Kubernetes node, and production platform depends on systemd to bring the operating system to life and keep it running reliably.

Master systemd and you gain control over the lifecycle of the entire operating system.
