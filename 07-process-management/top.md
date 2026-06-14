# Linux `top` Command — Real-Time Process Monitoring Master Guide

> If `ps` is a photograph of your Linux system,
>
> then `top` is a live video stream.
>
> `top` is one of the most important Linux monitoring tools ever created.
>
> It allows administrators to see:
>
> - CPU usage
> - Memory usage
> - Running processes
> - Threads
> - Load averages
> - System pressure
> - Performance bottlenecks
>
> in real time.
>
> Every Linux administrator, DevOps engineer, SRE, cloud engineer, database administrator, and performance engineer should master `top`.

---

# Learning Objectives

After completing this guide, you should understand:

```text
✓ What top is
✓ How top works internally
✓ CPU monitoring
✓ Memory monitoring
✓ Load Average
✓ Process monitoring
✓ Thread monitoring
✓ Interactive commands
✓ Process management
✓ Performance troubleshooting
✓ Production debugging
✓ Container monitoring
✓ Interview concepts
```

---

# What is `top`?

`top` is a real-time system monitoring tool.

Unlike:

```bash
ps
```

which shows:

```text
One Snapshot
```

`top` continuously updates.

Visual:

```text
ps

📷 Photo



top

🎥 Live Video
```

---

# Why top Exists

Suppose a server becomes slow.

Questions:

```text
Who is using CPU?
Who is consuming memory?
Which process is stuck?
Is the system overloaded?
```

`top` helps answer these immediately.

---

# Child-Friendly Explanation

Imagine a classroom.

Principal wants to know:

```text
Who is studying right now?
Who is sleeping?
Who is talking?
Who is using resources?
```

A photograph helps once.

A live CCTV camera helps continuously.

```text
top = Linux CCTV Camera
```

---

# Starting top

Run:

```bash
top
```

Example:

```text
top - 15:30:25 up 5 days,  3:21,
1 user,
load average: 0.50, 0.75, 1.10

Tasks: 235 total,
1 running,
234 sleeping,
0 stopped,
0 zombie

%Cpu(s): 5.0 us,
2.0 sy,
0.0 ni,
92.0 id,
1.0 wa

MiB Mem:
16000 total,
8000 free,
4000 used,
4000 buff/cache

MiB Swap:
4000 total,
4000 free,
0 used
```

---

# The Big Picture

```text
+--------------------------------+
| System Summary                 |
+--------------------------------+

+--------------------------------+
| CPU Information                |
+--------------------------------+

+--------------------------------+
| Memory Information             |
+--------------------------------+

+--------------------------------+
| Process Table                  |
+--------------------------------+
```

---

# How top Works Internally

Like `ps`, top reads:

```text
/proc
```

Visual:

```text
Kernel

   │

   ▼

 /proc

   │

   ▼

 top
```

But unlike ps:

```text
Repeatedly Reads Data
```

every few seconds.

---

# Refresh Rate

Default:

```text
3 Seconds
```

Visual:

```text
Read

Wait

Read

Wait

Read
```

---

# Change Refresh Rate

Start:

```bash
top -d 1
```

Updates every:

```text
1 Second
```

---

# Understanding the Header

Example:

```text
top - 15:30:25
```

---

## Current Time

```text
15:30:25
```

Current system time.

---

## Uptime

Example:

```text
up 5 days, 3:21
```

Meaning:

```text
System running for:

5 Days
3 Hours
21 Minutes
```

---

## Logged-in Users

Example:

```text
1 user
```

Means:

```text
One active login session
```

---

# Load Average

One of the most misunderstood Linux concepts.

Example:

```text
load average:
0.50, 0.75, 1.10
```

---

# What Is Load Average?

Measures:

```text
How busy system is.
```

Includes:

```text
Running Processes
Runnable Processes
Waiting Processes
```

---

# Three Numbers

Example:

```text
0.50
0.75
1.10
```

Represent:

```text
1 Minute Average

5 Minute Average

15 Minute Average
```

---

# Child-Friendly Example

Imagine:

```text
1 Cashier
```

Load:

```text
1 Customer
```

Perfect.

Load Average:

```text
1.0
```

---

# Example

```text
4 CPU Cores
```

Healthy load:

```text
4.0
```

Approximate full utilization.

---

# Load Interpretation

---

## Single Core

```text
1.0 = Fully Busy

2.0 = Overloaded
```

---

## 4-Core CPU

```text
4.0 = Fully Busy

8.0 = Overloaded
```

---

## 16-Core Server

```text
16.0 = Fully Busy

32.0 = Overloaded
```

---

# Tasks Section

Example:

```text
Tasks:
235 total,
1 running,
234 sleeping,
0 stopped,
0 zombie
```

---

# Total

```text
235 total
```

Total processes.

---

# Running

```text
1 running
```

Currently executing.

---

# Sleeping

```text
234 sleeping
```

Waiting for events.

Most Linux processes are sleeping.

---

# Stopped

```text
0 stopped
```

Paused processes.

---

# Zombie

```text
0 zombie
```

Exited but not cleaned.

---

# Visual

```text
Processes

Running

Sleeping

Stopped

Zombie
```

---

# CPU Section

Example:

```text
%Cpu(s):
5 us,
2 sy,
0 ni,
92 id,
1 wa
```

This is one of the most important sections.

---

# us (User CPU)

Time spent running:

```text
User Applications
```

Examples:

```text
Chrome

Python

Java

Node.js
```

---

# sy (System CPU)

Time spent in:

```text
Kernel
```

Examples:

```text
System Calls

Scheduling

Memory Management
```

---

# ni (Nice)

CPU used by:

```text
Processes With Nice Values
```

Usually small.

---

# id (Idle)

Unused CPU.

Example:

```text
92%
```

Meaning:

```text
CPU mostly idle.
```

---

# wa (I/O Wait)

Extremely important.

Time CPU waits for:

```text
Disk

Storage

Network Filesystems
```

---

# High wa Means?

Possible:

```text
Slow SSD

Slow HDD

Storage Failure

Database Bottleneck
```

---

# Example

```text
wa = 40%
```

Serious investigation needed.

---

# hi (Hardware Interrupts)

CPU time handling:

```text
Hardware Devices
```

Examples:

```text
Network Cards

Disk Controllers
```

---

# si (Software Interrupts)

Time spent handling:

```text
Software Interrupts
```

Kernel internal events.

---

# st (Steal Time)

Mostly virtual machines.

Meaning:

```text
Hypervisor took CPU away.
```

Example:

```text
AWS
Azure
VMware
KVM
```

---

# High Steal Time

Example:

```text
st = 20%
```

Means:

```text
VM wants CPU

Hypervisor busy elsewhere
```

---

# Memory Section

Example:

```text
MiB Mem:

16000 total

8000 free

4000 used

4000 buff/cache
```

---

# Total Memory

Installed RAM.

Example:

```text
16GB
```

---

# Used Memory

Memory actively used.

Example:

```text
4GB
```

---

# Free Memory

Unused RAM.

Example:

```text
8GB
```

---

# Buffers/Cache

Linux aggressively caches data.

Visual:

```text
RAM

 ├── Used
 ├── Free
 └── Cache
```

---

# Important Rule

Many beginners think:

```text
Low Free Memory = Problem
```

Wrong.

Linux intentionally uses RAM for cache.

---

# Swap Section

Example:

```text
Swap:

4000 total

0 used
```

---

# What is Swap?

Disk used as emergency memory.

Visual:

```text
RAM Full

      │

      ▼

Swap
```

---

# High Swap Usage

May indicate:

```text
Memory Pressure

Insufficient RAM

Memory Leak
```

---

# Process Table

The bottom section.

Example:

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
```

---

# PID

Process ID.

---

# USER

Process owner.

---

# PR

Priority.

---

# NI

Nice value.

Range:

```text
-20 → Highest Priority

 19 → Lowest Priority
```

---

# VIRT

Virtual Memory.

Equivalent to:

```text
VSZ
```

---

# RES

Resident Memory.

Actual RAM usage.

Equivalent to:

```text
RSS
```

---

# SHR

Shared memory.

Libraries and shared pages.

---

# S

Process state.

Examples:

```text
R

S

D

T

Z
```

---

# %CPU

Current CPU usage.

---

# %MEM

Current memory usage.

---

# TIME+

Total CPU time consumed.

---

# COMMAND

Process name.

---

# Interactive Commands

One of top's most powerful features.

---

# h

Help.

Press:

```text
h
```

Displays commands.

---

# q

Quit.

```text
q
```

---

# k

Kill process.

Press:

```text
k
```

Enter:

```text
PID
```

Send signal.

---

# Example

```text
k

1234

15
```

Sends:

```text
SIGTERM
```

---

# r

Renice process.

Press:

```text
r
```

Change priority.

---

# M

Sort by memory.

Visual:

```text
Highest Memory First
```

---

# P

Sort by CPU.

Visual:

```text
Highest CPU First
```

---

# T

Sort by CPU time.

Visual:

```text
Longest Running First
```

---

# c

Show full command line.

Before:

```text
python
```

After:

```text
python app.py --port 8080
```

---

# 1

Show per-CPU statistics.

Visual:

```text
CPU0

CPU1

CPU2

CPU3
```

Very useful on multi-core systems.

---

# Thread Monitoring

Show threads:

```text
H
```

Visual:

```text
Process

 ├── Thread
 ├── Thread
 └── Thread
```

---

# Why Useful?

Detect:

```text
Thread Explosion

Java Problems

Database Worker Issues
```

---

# CPU Troubleshooting Workflow

Step 1:

```bash
top
```

Step 2:

```text
Sort by CPU
```

Press:

```text
P
```

Step 3:

```text
Identify Top Consumer
```

---

# Memory Troubleshooting Workflow

Press:

```text
M
```

Identify:

```text
Largest Memory Users
```

---

# Zombie Investigation

Look:

```text
Tasks:

Zombie
```

If:

```text
Zombie > 0
```

Investigate parent processes.

---

# Container Monitoring

Containers are processes.

View:

```bash
top
```

You'll see:

```text
containerd

dockerd

runc
```

---

# Kubernetes Node Monitoring

View:

```text
kubelet

containerd

etcd
```

using top.

---

# Production Incident Example

User reports:

```text
Server Slow
```

Check:

```bash
top
```

Look at:

```text
Load Average

CPU

Memory

Swap

Top Processes
```

Usually enough to identify root cause quickly.

---

# top vs ps

| Feature | ps | top |
|----------|-----|-----|
| Snapshot | Yes | No |
| Real-Time | No | Yes |
| Interactive | No | Yes |
| Sorting | Limited | Dynamic |
| Monitoring | Limited | Excellent |

---

# top vs htop

| Feature | top | htop |
|----------|------|------|
| Installed Everywhere | Yes | No |
| Mouse Support | No | Yes |
| Better UI | No | Yes |
| Dependency-Free | Yes | No |

---

# Common Beginner Mistakes

---

## ❌ Load Average = CPU %

Wrong.

Different metric.

---

## ❌ High Memory Usage Is Bad

Not always.

Linux caches aggressively.

---

## ❌ Free RAM Must Be High

Wrong.

Unused RAM is wasted RAM.

---

## ❌ 100% CPU Means Broken

Sometimes normal.

Depends on workload.

---

## ❌ High wa Means CPU Problem

Usually storage problem.

---

# Interview Questions

## What does top do?

Provides real-time monitoring of processes and system resources.

---

## Difference Between ps and top?

ps:

```text
Snapshot
```

top:

```text
Live Monitoring
```

---

## What is Load Average?

Average number of runnable/waiting tasks over time.

---

## What does wa mean?

I/O Wait.

---

## What does st mean?

CPU time stolen by hypervisor.

---

## Which key sorts by CPU?

```text
P
```

---

## Which key sorts by memory?

```text
M
```

---

## Which key kills process?

```text
k
```

---

# Visual Summary

```text
top

 ├── System Time
 ├── Uptime
 ├── Load Average
 ├── Tasks
 ├── CPU Stats
 ├── Memory Stats
 ├── Swap Stats
 └── Process Table
```

Most Important Metrics:

```text
Load Average

us

sy

wa

st

Memory

Swap

%CPU

%MEM
```

Most Important Keys:

```text
P

M

T

H

1

c

k

q
```

---

# Final Takeaway

`top` is one of the most powerful troubleshooting tools in Linux.

When a system becomes:

```text
Slow

Unresponsive

Overloaded

Memory Hungry

CPU Bound
```

your first instinct should often be:

```bash
top
```

Mastering `top` gives you real-time visibility into what your Linux system is doing right now, making it one of the most valuable skills in Linux administration, DevOps, SRE, cloud operations, and performance engineering.
