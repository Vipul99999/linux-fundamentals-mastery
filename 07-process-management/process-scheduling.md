# Linux Process Scheduling

> Process Scheduling is the brain of multitasking.
>
> Every second, Linux must answer one critical question:
>
> ```text
> Which process should run next?
> ```
>
> The component responsible for making this decision is called the **Scheduler**.
>
> Without scheduling:
>
> - Your browser would freeze
> - Video playback would stutter
> - Databases would become unresponsive
> - Docker containers would fight for CPU
> - Kubernetes nodes would become unusable
>
> Modern Linux scheduling is one of the most sophisticated scheduling systems ever built.

---

# Child-Friendly Explanation

Imagine a teacher in a classroom.

```text
30 Students
1 Teacher
```

All students want help.

Teacher decides:

```text
Who gets attention next?
How long?
Who has waited too long?
Who needs urgent help?
```

The teacher is acting like a Linux scheduler.

---

# What is Process Scheduling?

Process Scheduling is the process of deciding:

```text
Which process gets CPU?
When?
For how long?
```

Visual:

```text
           CPU

            ▲
            │

    Scheduler Decision

            ▲
            │

 ┌───────────────────────┐
 │ Process A             │
 │ Process B             │
 │ Process C             │
 │ Process D             │
 └───────────────────────┘
```

---

# Why Scheduling Exists

Suppose:

```text
1000 Processes
8 CPU Cores
```

Can all run simultaneously?

```text
No
```

Linux must decide:

```text
Who runs now?
Who waits?
Who runs later?
```

---

# The Core Goal

Linux scheduling tries to balance:

```text
Fairness
Performance
Responsiveness
Throughput
Latency
Efficiency
```

Visual:

```text
           Scheduler

               │

 ┌─────────────┼─────────────┐

 Fairness   Performance   Responsiveness
```

---

# Real Example

You are using:

```text
Chrome
VS Code
Spotify
Terminal
Docker
MySQL
```

All appear active.

Visual:

```text
CPU Timeline

Chrome
VSCode
Spotify
MySQL
Chrome
Docker
VSCode
Chrome
```

The scheduler creates this illusion.

---

# Scheduler's Job

The scheduler continuously:

```text
Choose Process
Run Process
Pause Process
Switch Process
Repeat
```

Visual:

```text
Choose
   ↓
Run
   ↓
Evaluate
   ↓
Switch
   ↓
Repeat
```

Millions of times per second.

---

# What is a Run Queue?

Processes waiting for CPU are stored in a:

```text
Run Queue
```

Visual:

```text
Run Queue

+------------+
| Process A  |
+------------+
| Process B  |
+------------+
| Process C  |
+------------+
| Process D  |
+------------+
```

Scheduler selects from here.

---

# Real-Life Analogy

```text
Restaurant

Customers Waiting
      ↓
Queue
      ↓
Next Customer Served
```

Run Queue works similarly.

---

# Runnable vs Running

Runnable:

```text
Ready To Run
Waiting For CPU
```

Running:

```text
Currently Using CPU
```

Visual:

```text
Runnable Queue

A
B
C
D

      ↓

CPU

A
```

---

# Scheduling Cycle

```text
Process Enters Queue
        │
        ▼
Scheduler Selects
        │
        ▼
Process Runs
        │
        ▼
Time Slice Ends
        │
        ▼
Back To Queue
```

---

# Scheduling Goals

Linux tries to achieve:

---

## Goal 1: Fairness

Every process should get CPU time.

Bad:

```text
Process A = 99%
Others = 1%
```

Good:

```text
Process A = 25%
Process B = 25%
Process C = 25%
Process D = 25%
```

---

## Goal 2: Responsiveness

Interactive applications should feel fast.

Example:

```text
Mouse Click
Keyboard Input
Browser Interaction
```

Linux prioritizes responsiveness.

---

## Goal 3: Throughput

Maximize work completed.

Example:

```text
Database Queries
Web Requests
Batch Jobs
```

More completed work = better throughput.

---

## Goal 4: Low Latency

Latency means:

```text
Time Before Process Starts Running
```

Lower is better.

Example:

```text
Click Browser
Instant Response
```

---

# Historical Linux Schedulers

Linux evolved through many schedulers.

---

## O(n) Scheduler

Early Linux.

Problem:

```text
More Processes
More Work
```

Not scalable.

---

## O(1) Scheduler

Linux 2.6 introduced:

```text
O(1)
```

Meaning:

```text
Decision Time Constant
```

Even with many processes.

Huge improvement.

---

## Completely Fair Scheduler (CFS)

Modern Linux default scheduler.

Introduced:

```text
Linux 2.6.23
```

Still forms the basis of modern scheduling.

---

# What is CFS?

CFS means:

```text
Completely Fair Scheduler
```

Goal:

```text
Give every process fair CPU access
```

Visual:

```text
CPU Time

Process A ████
Process B ████
Process C ████
Process D ████
```

---

# Child-Friendly Explanation

Imagine:

```text
4 Children
1 Cake
```

Fair sharing:

```text
25%
25%
25%
25%
```

Not:

```text
90%
5%
3%
2%
```

CFS behaves similarly.

---

# Virtual Runtime (vruntime)

CFS tracks:

```text
Virtual Runtime
```

Abbreviated:

```text
vruntime
```

This is the core of CFS.

---

# What is vruntime?

Measures:

```text
How much CPU time has this process received?
```

Visual:

```text
Process A = 100

Process B = 50

Process C = 20
```

Process C gets priority.

Why?

```text
Received least CPU time
```

---

# Scheduling Decision

CFS chooses:

```text
Process With Lowest vruntime
```

Visual:

```text
A = 500

B = 300

C = 100
      ▲
      │
 Selected
```

---

# Why This Works

Processes that run more:

```text
vruntime increases
```

Processes that wait:

```text
vruntime remains low
```

Result:

```text
Natural Fairness
```

---

# Red-Black Tree

CFS stores runnable processes in:

```text
Red-Black Tree
```

Visual:

```text
             200
            /   \
         100     400
        /       /   \
      50      300   500
```

Processes sorted by:

```text
vruntime
```

Left-most node:

```text
Lowest vruntime
```

gets CPU.

---

# Why Red-Black Tree?

Benefits:

```text
Fast Insert
Fast Delete
Fast Search
Balanced Structure
```

Complexity:

```text
O(log n)
```

---

# Multi-Core Scheduling

Modern CPUs:

```text
Core 1
Core 2
Core 3
Core 4
...
```

Each core has:

```text
Own Run Queue
```

Visual:

```text
Core 1 Queue

A
B

Core 2 Queue

C
D
```

---

# Load Balancing

Linux balances work across CPUs.

Bad:

```text
Core 1 = 100% Busy
Core 2 = Idle
```

Good:

```text
Core 1 = 50%
Core 2 = 50%
```

---

# Scheduler Domains

Linux groups CPUs into domains.

Example:

```text
Socket
NUMA Node
CPU Group
```

Used for intelligent balancing.

---

# NUMA Awareness

Modern servers:

```text
Multiple CPU Sockets
```

Memory access costs differ.

Visual:

```text
CPU 1 → Local Memory

CPU 2 → Remote Memory
```

Linux scheduler considers this.

---

# CPU Affinity

Affinity means:

```text
Run process on specific CPU
```

Example:

```bash
taskset -c 0 nginx
```

Visual:

```text
Process
   │
   ▼
CPU 0 Only
```

---

# Why CPU Affinity Helps

Benefits:

```text
Better Cache Usage
Lower Latency
Higher Performance
```

Useful for:

```text
Databases
Trading Systems
Gaming Servers
```

---

# Real-Time Scheduling

Some systems require strict timing.

Examples:

```text
Aircraft Systems
Medical Devices
Industrial Controllers
Robotics
```

Linux supports:

```text
Real-Time Scheduling
```

---

# Scheduling Classes

Linux scheduling classes:

```text
STOP
DEADLINE
REALTIME
FAIR (CFS)
IDLE
```

Priority Order:

```text
STOP

DEADLINE

REALTIME

FAIR

IDLE
```

---

# CFS Scheduling Class

Most applications use:

```text
SCHED_NORMAL
```

Handled by:

```text
CFS
```

Examples:

```text
Chrome
VSCode
Docker
Node.js
Python
```

---

# Starvation

Starvation occurs when:

```text
Process Never Gets CPU
```

Visual:

```text
A Runs
B Runs
C Runs

D Waits Forever
```

Bad scheduler behavior.

Modern Linux minimizes starvation.

---

# Context Switching and Scheduling

Scheduling decides:

```text
Who Runs Next
```

Context Switching performs:

```text
Actual Switch
```

Visual:

```text
Scheduler
    │
Chooses B
    │
    ▼
Context Switch
    │
    ▼
Process B Runs
```

---

# Docker and Scheduling

Containers are:

```text
Linux Processes
```

Visual:

```text
Docker Container
       │
       ▼
Linux Process
       │
       ▼
CFS Scheduler
```

Containers participate in normal scheduling.

---

# Kubernetes Scheduling vs Linux Scheduling

Important distinction.

---

## Kubernetes Scheduler

Decides:

```text
Which Node Runs Pod?
```

---

## Linux Scheduler

Decides:

```text
Which Process Gets CPU?
```

Visual:

```text
Kubernetes Scheduler
        │
        ▼
Node Selection

Linux Scheduler
        │
        ▼
CPU Selection
```

---

# Viewing Scheduling Information

---

## Process Priority

```bash
ps -eo pid,ni,comm
```

---

## CPU Usage

```bash
top
```

---

## Scheduler Statistics

```bash
pidstat
```

---

## CPU Information

```bash
mpstat
```

---

## Affinity

```bash
taskset -p PID
```

---

# Common Beginner Mistakes

---

## ❌ Scheduler Runs Processes

Not exactly.

Scheduler:

```text
Chooses
```

Context Switch:

```text
Runs Choice
```

---

## ❌ More CPUs = No Scheduling

Wrong.

Every core still schedules tasks.

---

## ❌ Fair Means Equal

Wrong.

Fairness considers:

```text
Priority
Weight
Nice Value
Scheduling Policy
```

---

## ❌ Containers Have Their Own Scheduler

Wrong.

Containers use Linux scheduling.

---

# Production Examples

---

## Web Server

```text
Nginx
```

Scheduler balances:

```text
Workers
Connections
Requests
```

---

## Database

```text
PostgreSQL
MySQL
```

Scheduler manages:

```text
Query Workers
Background Processes
Replication Threads
```

---

## Kubernetes Node

```text
Hundreds Of Containers
Thousands Of Threads
```

Linux scheduler manages all.

---

# Interview Questions

---

## What is Process Scheduling?

The mechanism that determines which process gets CPU time.

---

## What is a Run Queue?

Queue containing runnable processes waiting for CPU.

---

## What is CFS?

Completely Fair Scheduler.

Default Linux scheduler.

---

## What is vruntime?

Virtual runtime used by CFS to track CPU usage fairness.

---

## Why does CFS use a Red-Black Tree?

Efficient:

```text
Insert
Delete
Search
```

operations.

---

## What is CPU Affinity?

Binding a process to specific CPU cores.

---

## Difference Between Scheduler and Context Switch?

Scheduler:

```text
Chooses Next Process
```

Context Switch:

```text
Performs Transition
```

---

# Quick Revision

```text
Scheduler
    │
    ▼
Chooses Process
    │
    ▼
Context Switch
    │
    ▼
Process Runs
```

Key Concepts:

✓ Run Queue

✓ Runnable Process

✓ Fairness

✓ Throughput

✓ Latency

✓ Responsiveness

✓ CFS

✓ vruntime

✓ Red-Black Tree

✓ Multi-Core Scheduling

✓ CPU Affinity

✓ NUMA Awareness

✓ Real-Time Scheduling

Remember:

"The Scheduler is the Traffic Controller of the CPU."

Without it, modern Linux systems, cloud platforms, containers, databases, and applications could not share CPU resources efficiently.
