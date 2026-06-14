# Linux CPU Schedulers

> If Process Scheduling answers:
>
> ```text
> "Who should run next?"
> ```
>
> CPU Schedulers answer:
>
> ```text
> "Which scheduling algorithm and policy should be used?"
> ```
>
> Understanding Linux CPU schedulers is essential for:
>
> - System Administrators
> - DevOps Engineers
> - SREs
> - Kernel Engineers
> - Performance Engineers
> - Cloud Engineers
> - Database Administrators
> - Container Platform Engineers
>
> Modern Linux scheduling is used by:
>
> ```text
> Google
> AWS
> Azure
> Meta
> Netflix
> Kubernetes
> Docker
> PostgreSQL
> MySQL
> Redis
> ```
>
> Every Linux server relies on CPU schedulers.

---

# Big Picture

Before learning scheduler types, understand the hierarchy.

```text
                 Linux Scheduler

                        │
                        ▼

              Scheduling Classes

                        │
                        ▼

 ┌─────────────┬──────────────┬──────────────┬──────────────┐
 │             │              │              │
 ▼             ▼              ▼              ▼

 STOP       DEADLINE      REALTIME         FAIR
                               │
                               ▼
                        FIFO / RR
```

The Linux kernel checks scheduler classes in priority order.

---

# Why Multiple Schedulers Exist

Not all workloads are the same.

Example:

```text
Music Player
Database
Robot Controller
Stock Trading System
Web Server
```

Should they all be treated identically?

```text
No
```

Different workloads require different scheduling behavior.

---

# Real World Example

Imagine a hospital.

```text
Emergency Patient
Regular Patient
Scheduled Checkup
Hospital Staff
```

Everyone has different priority.

Linux scheduling works similarly.

---

# Linux Scheduling Classes

Modern Linux contains multiple scheduling classes.

Priority order:

```text
STOP

DEADLINE

REALTIME

FAIR

IDLE
```

Higher class always wins.

Visual:

```text
Highest Priority
        │

     STOP

        │

   DEADLINE

        │

   REALTIME

        │

      FAIR

        │

      IDLE

        │
Lowest Priority
```

---

# Class 1: STOP Scheduler

Highest priority class.

Used internally by kernel.

Normal users rarely interact with it.

Purpose:

```text
Critical CPU Stop Operations
```

Example:

```text
CPU Hotplug
Kernel Maintenance
Migration Tasks
```

Visual:

```text
STOP
  │
Everything Else Waits
```

---

# Why STOP Exists

Sometimes Linux must:

```text
Pause Everything
Perform Critical Work
Resume Everything
```

STOP scheduler provides this ability.

---

# Class 2: DEADLINE Scheduler

One of Linux's most advanced schedulers.

Policy:

```text
SCHED_DEADLINE
```

Goal:

```text
Finish before deadline
```

---

# Real-Life Example

Suppose:

```text
Brake System
Must Respond Within 5ms
```

Missing deadline:

```text
Potential Disaster
```

DEADLINE scheduling is designed for such systems.

---

# Examples

Used in:

```text
Aircraft Systems
Industrial Robots
Medical Devices
Automotive Systems
Telecommunications
```

---

# DEADLINE Concept

Each task specifies:

```text
Runtime
Deadline
Period
```

Visual:

```text
Task

Runtime = 2ms

Deadline = 10ms

Period = 20ms
```

Meaning:

```text
Need 2ms CPU
Must Finish Within 10ms
Repeats Every 20ms
```

---

# Deadline Visualization

```text
Time →

|------ Deadline ------|

Task Starts
      │
      ▼
 Execute
      │
      ▼
 Finish Before Deadline
```

---

# Why DEADLINE is Powerful

Guarantees:

```text
Predictable Timing
```

Unlike normal scheduling.

Perfect for:

```text
Hard Real-Time Systems
```

---

# Class 3: REALTIME Scheduler

Used for time-sensitive workloads.

Policies:

```text
SCHED_FIFO

SCHED_RR
```

---

# Real-Time Doesn't Mean Fast

Many beginners think:

```text
Real-Time = High Speed
```

Wrong.

Real-Time means:

```text
Predictable Response
```

---

# Real-Time Example

Robot Arm:

```text
Move Every 5ms
```

Requirement:

```text
Consistency
```

Not maximum speed.

---

# REALTIME Priority Range

Linux real-time priorities:

```text
1 - 99
```

Visual:

```text
99 = Highest RT Priority

1  = Lowest RT Priority
```

Higher value wins.

---

# SCHED_FIFO

FIFO means:

```text
First In First Out
```

Behavior:

```text
Run Until:
- Finish
- Block
- Yield
```

Visual:

```text
Task A Arrives

Runs

Runs

Runs

Runs

Runs

Until Finished
```

No automatic time slice.

---

# FIFO Example

```text
Task A Priority 80

Task B Priority 70
```

Result:

```text
Task A Runs First
```

Task B waits.

---

# Danger of FIFO

Badly written task:

```text
Infinite Loop
```

May monopolize CPU.

Visual:

```text
Task A
  │
Never Stops
  │
CPU Locked
```

---

# SCHED_RR

RR means:

```text
Round Robin
```

Similar to FIFO but:

```text
Time Slice Added
```

---

# Visualization

```text
Task A

Task B

Task C
```

CPU:

```text
A → B → C → A → B → C
```

---

# Why RR Exists

Prevents:

```text
CPU Monopoly
```

Provides fairness among RT tasks.

---

# FIFO vs RR

| Feature | FIFO | RR |
|----------|------|----|
| Time Slice | No | Yes |
| Fairness | Lower | Higher |
| Predictability | High | High |
| Starvation Risk | Higher | Lower |

---

# Class 4: FAIR Scheduler

Most common Linux scheduler.

Policy:

```text
SCHED_NORMAL
```

Managed by:

```text
CFS
```

(Completely Fair Scheduler)

---

# Everyday Applications

Most applications use FAIR scheduling.

Examples:

```text
Chrome
Firefox
VS Code
Node.js
Python
Java
Docker
Nginx
MySQL
Redis
```

---

# Goal of FAIR Scheduler

Provide:

```text
Fair CPU Distribution
```

Visual:

```text
CPU Time

A ████

B ████

C ████

D ████
```

---

# Completely Fair Scheduler (CFS)

For many years Linux used:

```text
CFS
```

Core idea:

```text
Processes that received less CPU
get priority.
```

---

# Virtual Runtime

CFS tracks:

```text
vruntime
```

Meaning:

```text
How much CPU has process consumed?
```

Example:

```text
Process A = 100

Process B = 50

Process C = 10
```

Scheduler chooses:

```text
Process C
```

Because it received least CPU.

---

# CFS Red-Black Tree

Runnable processes stored in:

```text
Red-Black Tree
```

Visual:

```text
          50
         /  \
       20   100
      /
     10
```

Left-most node:

```text
Lowest vruntime
```

Runs next.

---

# Modern Linux: EEVDF

Recent Linux kernels (6.x+) introduced:

```text
EEVDF
```

Earliest Eligible Virtual Deadline First

---

# Why EEVDF?

CFS worked well.

However:

```text
Latency
Interactivity
Fairness
```

could be improved.

---

# EEVDF Goals

Improve:

```text
Desktop Responsiveness

Gaming

Interactive Applications

Mixed Workloads
```

---

# Simplified View

Instead of focusing only on:

```text
Past CPU Usage
```

EEVDF also considers:

```text
Future Scheduling Deadlines
```

Result:

```text
Better Responsiveness
```

---

# Why This Matters

Example:

```text
Web Browser
Video Call
Game
```

Need fast reactions.

EEVDF improves user experience.

---

# Class 5: IDLE Scheduler

Lowest priority.

Runs only when:

```text
Nothing Else Needs CPU
```

Visual:

```text
Normal Tasks
      │
Finished
      │
      ▼
Idle Tasks Run
```

---

# Scheduling Policies

User-visible policies:

```text
SCHED_NORMAL

SCHED_BATCH

SCHED_IDLE

SCHED_FIFO

SCHED_RR

SCHED_DEADLINE
```

---

# SCHED_NORMAL

Default.

Used by:

```text
Almost Everything
```

Balanced scheduling.

---

# SCHED_BATCH

Designed for:

```text
Batch Processing
```

Examples:

```text
Log Analysis
Data Processing
Backups
```

Prioritizes throughput.

---

# SCHED_IDLE

Extremely low priority.

Example:

```text
Background Cleanup
```

Runs only when CPU is free.

---

# Scheduler Priorities

Two worlds exist:

```text
Normal Priorities

Real-Time Priorities
```

---

# Normal Priorities

Controlled by:

```bash
nice
```

Range:

```text
-20 → Highest Priority

 19 → Lowest Priority
```

---

# Example

```bash
nice -n -10 myapp
```

Higher scheduling weight.

---

# Real-Time Priorities

Range:

```text
1 → 99
```

Used with:

```text
FIFO
RR
```

---

# Viewing Scheduling Policy

```bash
chrt -p PID
```

Example:

```text
policy = SCHED_OTHER
priority = 0
```

---

# Setting Real-Time Policy

Example:

```bash
sudo chrt -f 80 myapp
```

Meaning:

```text
FIFO

Priority 80
```

---

# CPU Affinity and Scheduling

Scheduler also respects:

```text
CPU Affinity
```

Example:

```bash
taskset -c 0 myapp
```

Process restricted to:

```text
CPU 0
```

---

# Why Affinity Matters

Benefits:

```text
Cache Reuse

Lower Latency

Predictable Performance
```

Used heavily in:

```text
Databases

Trading Systems

Telecommunications
```

---

# Containers and Scheduling

Docker containers:

```text
Linux Processes
```

Therefore:

```text
Linux Scheduler
```

manages them.

---

# Kubernetes and Scheduling

Important distinction.

---

## Kubernetes Scheduler

Chooses:

```text
Which Node Runs Pod?
```

---

## Linux Scheduler

Chooses:

```text
Which CPU Runs Process?
```

---

# Production Example

Node:

```text
100 Pods

2000 Processes

5000 Threads
```

Linux scheduler manages actual CPU allocation.

---

# Troubleshooting Scheduler Problems

Symptoms:

```text
High CPU

Latency

Starvation

Slow Applications
```

Tools:

```bash
top
```

```bash
htop
```

```bash
pidstat
```

```bash
vmstat
```

```bash
perf
```

---

# Common Beginner Mistakes

## ❌ Real-Time Means Faster

Wrong.

Means:

```text
More Predictable
```

---

## ❌ FIFO Is Always Better

Wrong.

Can starve other processes.

---

## ❌ Docker Has Its Own CPU Scheduler

Wrong.

Uses Linux scheduler.

---

## ❌ Nice Values Affect Real-Time Tasks

Wrong.

Nice affects normal scheduling only.

---

# Interview Questions

## What are Linux scheduling classes?

```text
STOP
DEADLINE
REALTIME
FAIR
IDLE
```

---

## What is CFS?

Completely Fair Scheduler.

Default Linux scheduler.

---

## What is EEVDF?

Earliest Eligible Virtual Deadline First.

Modern Linux scheduling improvement over traditional CFS behavior.

---

## Difference Between FIFO and RR?

FIFO:

```text
No Time Slice
```

RR:

```text
Uses Time Slice
```

---

## What is SCHED_DEADLINE?

Scheduler for deadline-based real-time workloads.

---

## What command displays scheduling policy?

```bash
chrt -p PID
```

---

## What is a nice value?

User-controlled priority adjustment for normal processes.

---

# Quick Revision

```text
Linux Scheduling Classes

STOP
  ↓
DEADLINE
  ↓
REALTIME
  ↓
FAIR
  ↓
IDLE
```

Policies:

```text
SCHED_NORMAL

SCHED_BATCH

SCHED_IDLE

SCHED_FIFO

SCHED_RR

SCHED_DEADLINE
```

Key Technologies:

✓ CFS

✓ EEVDF

✓ FIFO

✓ RR

✓ DEADLINE

✓ CPU Affinity

✓ Nice Values

✓ Real-Time Priorities

Remember:

"The scheduler decides who gets the CPU; the scheduling policy decides how that decision is made."
