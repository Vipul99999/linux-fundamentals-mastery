# Linux Process Management

> Processes are the heart of Linux. Every application, service, container, shell, daemon, and system component ultimately runs as one or more processes.

Understanding process management is one of the most important skills for:

```text
Linux Administrators

DevOps Engineers

Site Reliability Engineers (SRE)

Backend Developers

Platform Engineers

Security Engineers

Cloud Engineers
```

This module takes you from:

```text
"What is a process?"
```

to:

```text
"How the Linux kernel schedules thousands of processes across CPUs while isolating containers using cgroups and namespaces."
```

---

# Learning Objectives

After completing this module, you will understand:

✅ What a process is

✅ Process lifecycle

✅ PID and PPID

✅ Process states

✅ Process creation

✅ Fork and Exec

✅ Zombie processes

✅ Orphan processes

✅ Threads

✅ Signals

✅ Job control

✅ Scheduling

✅ Nice and Renice

✅ CPU affinity

✅ systemd

✅ cgroups

✅ Namespaces

✅ Process monitoring

✅ Performance troubleshooting

✅ Security implications

✅ Container process isolation

---

# Module Structure

```text
07-process-management/
│
├── README.md
│
├── process-basics.md
├── process-lifecycle.md
├── pid-and-ppid.md
├── process-states.md
├── process-creation.md
├── fork-and-exec.md
├── zombie-processes.md
├── orphan-processes.md
├── threads.md
│
├── signals.md
├── signal-handling.md
├── job-control.md
├── foreground-background.md
│
├── scheduling-basics.md
├── nice-and-renice.md
├── cpu-affinity.md
├── realtime-scheduling.md
│
├── process-monitoring.md
├── ps-command.md
├── top-and-htop.md
├── pstree.md
├── pidof-pgrep-pkill.md
├── lsof.md
├── strace.md
├── perf-basics.md
│
├── systemd-basics.md
├── systemctl.md
├── journald.md
├── service-management.md
│
├── cgroups.md
├── namespaces.md
├── container-processes.md
│
├── process-security.md
├── process-hardening.md
│
├── troubleshooting.md
├── common-mistakes.md
├── real-world-case-studies.md
├── flowcharts.md
├── labs.md
├── visuals.md
│
├── interview-questions.md
└── references.md
```

---

# Why Processes Matter

Everything in Linux is built around processes.

Examples:

```text
Web Server

Database

Docker

Kubernetes

SSH

Shell

System Services
```

All are processes.

---

Visualization:

```text
Linux System
      │
      ▼
Processes
      │
      ▼
Applications
      │
      ▼
Services
      │
      ▼
Users
```

---

# What Is a Process?

A process is:

```text
A running instance of a program.
```

Example:

Program:

```text
/bin/bash
```

Running:

```text
bash
```

becomes:

```text
Process
```

---

Visualization

```text
Program
      │
      ▼
Execution
      │
      ▼
Process
```

---

# Program vs Process

Program:

```text
Static

Stored On Disk
```

---

Process:

```text
Running

Stored In Memory
```

---

Visualization

```text
Program
 (/usr/bin/python)
         │
         ▼
     Execute
         │
         ▼
Process
 (PID 1234)
```

---

# Process Hierarchy

Linux processes form a tree.

Visualization:

```text
systemd (PID 1)
│
├── sshd
│
├── nginx
│
│   ├── worker1
│   ├── worker2
│
├── mysql
│
└── bash
     │
     └── vim
```

Every process except PID 1 has:

```text
Parent Process
```

---

# Process Lifecycle

```text
Created
   │
   ▼
Ready
   │
   ▼
Running
   │
   ▼
Waiting
   │
   ▼
Running
   │
   ▼
Terminated
```

---

# Process States

Linux processes can be:

```text
Running

Sleeping

Stopped

Zombie

Dead
```

---

Visualization

```text
Process
   │
   ├── Running
   ├── Sleeping
   ├── Stopped
   └── Zombie
```

---

# Signals

Signals are software interrupts.

Examples:

```text
SIGTERM

SIGKILL

SIGSTOP

SIGCONT
```

---

Visualization

```text
User
 │
 ▼
kill command
 │
 ▼
Signal
 │
 ▼
Process
```

---

# Job Control

Shells support:

```text
Foreground Jobs

Background Jobs
```

Commands:

```bash
jobs

bg

fg
```

---

Visualization

```text
Shell
 │
 ├── Foreground
 │
 └── Background
```

---

# Scheduling

The kernel decides:

```text
Who Runs

When

For How Long
```

---

Visualization

```text
CPU
 │
 ▼
Scheduler
 │
 ▼
Process Queue
```

---

# Process Monitoring

Critical commands:

```bash
ps

top

htop

pstree

lsof
```

---

Visualization

```text
System
 │
 ▼
Monitoring Tools
 │
 ▼
Process Information
```

---

# systemd

Modern Linux init system.

Responsible for:

```text
Boot

Services

Logging

Dependencies
```

---

Visualization

```text
systemd
 │
 ├── nginx.service
 ├── mysql.service
 └── ssh.service
```

---

# cgroups

Control Groups.

Used for:

```text
CPU Limits

Memory Limits

I/O Limits
```

---

Visualization

```text
Processes
      │
      ▼
cgroup
      │
      ▼
Resource Limits
```

---

# Namespaces

Provide isolation.

Types:

```text
PID

Network

Mount

User

IPC
```

---

Visualization

```text
Namespace A
     │
     ▼
Processes
```

```text
Namespace B
     │
     ▼
Processes
```

---

# Containers

Containers are:

```text
Processes
+
Namespaces
+
cgroups
```

---

Visualization

```text
Container
 │
 ├── Processes
 ├── Namespace
 └── cgroup
```

---

# Security Perspective

Processes are often targeted by:

```text
Privilege Escalation

Code Injection

Process Hijacking

Resource Abuse
```

Understanding processes improves security.

---

# Troubleshooting Perspective

Many outages involve:

```text
Hung Processes

Zombie Processes

CPU Spikes

Memory Leaks

Deadlocks
```

Process knowledge is essential.

---

# Learning Roadmap

Beginner

```text
process-basics

pid-and-ppid

process-states
```

---

Intermediate

```text
signals

job-control

monitoring
```

---

Advanced

```text
scheduling

systemd

strace

perf
```

---

Expert

```text
cgroups

namespaces

containers

kernel internals
```

---

# Visual Learning Map

```text
Processes
     │
     ▼
Lifecycle
     │
     ▼
States
     │
     ▼
Signals
     │
     ▼
Scheduling
     │
     ▼
Monitoring
     │
     ▼
systemd
     │
     ▼
cgroups
     │
     ▼
Namespaces
     │
     ▼
Containers
```

---

# Real-World Applications

Understanding processes helps with:

```text
Server Administration

Cloud Infrastructure

Docker

Kubernetes

Performance Tuning

Security Auditing

Production Troubleshooting
```

---

# Module Outcome

By the end of this module, you will be able to:

```text
Analyze Processes

Debug Services

Understand Scheduling

Manage systemd

Troubleshoot Production Systems

Understand Containers Internally

Work With Linux Like A Professional
```

---

# Next File

Start with:

```text
process-basics.md
```

This file will explain:

- What a process actually is
- Process memory layout
- Program vs Process
- Process resources
- Kernel representation of processes
- Process metadata
- Process identifiers
- Real-world examples
- Internal Linux architecture

and forms the foundation for everything else in process management.
