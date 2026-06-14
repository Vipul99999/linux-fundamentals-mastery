# Linux Context Switching

> Context Switching is one of the most important concepts in Operating Systems, Linux, Cloud Computing, Containers, Databases, and High-Performance Systems.
>
> Without context switching:
>
> - No multitasking
> - No browsers
> - No Docker
> - No Kubernetes
> - No databases
> - No modern operating systems

---

# What is Context Switching?

A context switch occurs when the CPU stops executing one process (or thread) and starts executing another.

Think of it as:

```text
Pause Process A
Save Its Work

Start Process B
Continue Its Work
```

Visual:

```text
CPU

Process A Running
        │
        ▼
 Save State
        │
        ▼
 Load State
        │
        ▼
Process B Running
```

---

# Child-Friendly Explanation

Imagine you are doing:

```text
Math Homework
```

Suddenly:

```text
Teacher asks a question
```

You stop homework.

```text
Remember current page
Remember current answer
Remember current step
```

Then:

```text
Answer teacher
```

Later:

```text
Return to homework
Continue from exact point
```

That is exactly what Linux does.

---

# Why Context Switching Exists

Imagine:

```text
1000 Processes
1 CPU Core
```

Can all run simultaneously?

```text
No
```

Linux creates an illusion:

```text
Process A
Process B
Process C
Process D
```

all running together.

How?

```text
Context Switching
```

---

# The Big Picture

```text
             CPU

      Process A Running
               │
               ▼

         Context Switch

               │
               ▼

      Process B Running
```

This happens thousands or millions of times every second.

---

# Real Example

You open:

```text
Chrome
VS Code
Spotify
Terminal
Docker
```

Visual:

```text
CPU Timeline

| Chrome |
| VSCode |
| Docker |
| Chrome |
| Spotify |
| Terminal |
| Chrome |
```

The CPU rapidly switches between them.

To humans:

```text
Everything appears simultaneous
```

---

# What Does "Context" Mean?

Context means:

```text
Everything needed to resume execution later
```

Visual:

```text
Process Context

├── CPU Registers
├── Program Counter
├── Stack Pointer
├── Flags
├── Scheduling State
└── Memory State
```

---

# Simple Example

Suppose:

```c
a = 10;
b = 20;
c = a + b;
```

CPU currently executing:

```text
Instruction #3
```

Linux must remember:

```text
Current Instruction
Register Values
Stack State
```

Otherwise execution would restart incorrectly.

---

# What Gets Saved?

During a context switch:

```text
Program Counter
CPU Registers
Stack Pointer
Flags Register
Thread Information
Scheduling Data
```

Visual:

```text
Process A

+----------------+
| Program Counter|
+----------------+
| Registers      |
+----------------+
| Stack Pointer  |
+----------------+
| Flags          |
+----------------+
```

---

# Program Counter

Stores:

```text
Which instruction runs next?
```

Example:

```text
Instruction 500
```

Visual:

```text
Code

498
499
500 ← Current Position
501
502
```

Linux saves this location.

---

# CPU Registers

Registers contain temporary data.

Example:

```text
RAX
RBX
RCX
RDX
```

Visual:

```text
CPU

RAX = 100
RBX = 50
RCX = 200
```

Before switching:

```text
Save Registers
```

After switching back:

```text
Restore Registers
```

---

# Stack Pointer

Tracks current stack location.

Visual:

```text
Stack

+---------+
| Function|
+---------+
| Variable|
+---------+
| SP      |
+---------+
```

Linux saves this position.

---

# Where Is Context Stored?

Inside:

```text
PCB
```

Linux structure:

```c
task_struct
```

Visual:

```text
Running Process
       │
       ▼
 Save Context
       │
       ▼
 PCB (task_struct)
```

---

# Context Switch Step-by-Step

Imagine:

```text
Process A Running
```

Linux decides:

```text
Run Process B
```

Sequence:

```text
1. Interrupt Occurs

2. Save Process A Context

3. Store Context In PCB

4. Select Process B

5. Load Process B Context

6. Resume Process B
```

Visual:

```text
Process A
     │
     ▼
 Save Context
     │
     ▼
 Scheduler
     │
     ▼
 Load Context
     │
     ▼
 Process B
```

---

# Detailed Flow Diagram

```text
      Process A Running
              │
              ▼

      Save Registers

              │
              ▼

      Save Program Counter

              │
              ▼

      Save Stack Pointer

              │
              ▼

      Store In PCB

              │
              ▼

      Scheduler Chooses B

              │
              ▼

      Load B Context

              │
              ▼

      Process B Running
```

---

# Who Initiates Context Switching?

The Linux Scheduler.

Visual:

```text
Linux Scheduler

       │
       ▼

 Decide Next Process

       │
       ▼

 Context Switch
```

---

# Why Does a Context Switch Happen?

Several reasons.

---

# Reason 1: Time Slice Expired

Linux gives:

```text
Small CPU Time Slice
```

Example:

```text
5ms
```

After:

```text
Time Slice Ends
```

Scheduler switches.

Visual:

```text
Process A
     │
 5ms Used
     │
     ▼
Switch
```

---

# Reason 2: Process Sleeps

Example:

```text
Waiting For Disk
```

Visual:

```text
Process A
     │
Needs File
     │
     ▼
Sleep
     │
     ▼
Switch To Process B
```

---

# Reason 3: Higher Priority Process Arrives

Example:

```text
Background Sync Running
```

Then:

```text
Database Request Arrives
```

Scheduler may preempt.

Visual:

```text
Low Priority Process
        │
        ▼
High Priority Process Arrives
        │
        ▼
Switch
```

---

# Reason 4: Interrupt Occurs

Example:

```text
Keyboard Press
Network Packet
Disk Completion
```

CPU handles interrupt.

May switch processes.

---

# Voluntary Context Switch

Process chooses to give up CPU.

Example:

```text
Waiting For Disk
Waiting For Network
Waiting For User Input
```

Visual:

```text
Process
     │
Need Resource
     │
     ▼
Sleep
```

---

# Involuntary Context Switch

Kernel forces switch.

Example:

```text
Time Slice Expires
```

Visual:

```text
Running
     │
Time Limit Reached
     │
     ▼
Forced Switch
```

---

# Thread Context Switching

Context switching is not limited to processes.

Threads also switch.

Visual:

```text
Process

 ├── Thread 1
 ├── Thread 2
 └── Thread 3
```

Scheduler may switch:

```text
Thread 1 → Thread 2
```

---

# Process Switch vs Thread Switch

## Process Switch

More expensive.

Must change:

```text
Memory Space
Page Tables
Security Context
```

---

## Thread Switch

Less expensive.

Threads share:

```text
Memory
Files
Resources
```

Visual:

```text
Same Process
     │
     ▼
Thread Switch
```

Much faster.

---

# Modern CPU View

Single Core:

```text
CPU

A → B → C → D
```

Multi-Core:

```text
Core 1 → Process A
Core 2 → Process B
Core 3 → Process C
Core 4 → Process D
```

Context switches still happen on each core.

---

# Context Switching Cost

Many beginners think:

```text
Switching Is Free
```

Wrong.

Switching requires:

```text
Saving State
Loading State
Scheduler Work
Cache Effects
TLB Effects
```

---

# Why Too Many Context Switches Are Bad

Imagine:

```text
Work = 5ms
Switch = 2ms
```

Visual:

```text
Work
Switch
Work
Switch
Work
Switch
```

CPU wastes time switching.

---

# Cache Pollution

Modern CPUs use cache.

Visual:

```text
CPU Cache

Data For Process A
```

Switch:

```text
Process B Runs
```

Cache contents may become useless.

Result:

```text
More Cache Misses
Lower Performance
```

---

# TLB Impact

CPU stores memory translations in:

```text
TLB
```

Process switches may require:

```text
TLB Updates
```

More overhead.

---

# Real Production Example

Database Server:

```text
50000 Requests
```

Too many threads:

```text
Huge Context Switching
```

Symptoms:

```text
High CPU
Low Throughput
Poor Latency
```

---

# Docker and Context Switching

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
```

Container processes participate in normal context switching.

---

# Kubernetes Example

Pod:

```text
Nginx Container
```

Internally:

```text
Linux Process
```

Scheduler still performs:

```text
Context Switching
```

Containers do not eliminate it.

---

# Viewing Context Switches

## vmstat

```bash
vmstat 1
```

Example:

```text
cs
12000
```

Meaning:

```text
12000 Context Switches/Second
```

---

# pidstat

```bash
pidstat -w
```

Shows:

```text
Voluntary Context Switches
Involuntary Context Switches
```

---

# /proc

View process statistics:

```bash
cat /proc/PID/status
```

Example:

```text
voluntary_ctxt_switches
nonvoluntary_ctxt_switches
```

---

# top

Press:

```text
H
```

to view thread activity.

Useful for identifying switching-heavy applications.

---

# Performance Troubleshooting

High Context Switching May Mean:

```text
Too Many Threads
CPU Starvation
Lock Contention
Thread Thrashing
Improper Scheduling
```

---

# Common Beginner Mistakes

## ❌ Context Switch Means Process Exit

Wrong.

Process continues later.

---

## ❌ Process Restarts After Switch

Wrong.

Execution resumes exactly where it stopped.

---

## ❌ Switching Is Free

Wrong.

Switching consumes CPU time.

---

## ❌ Only Processes Switch

Wrong.

Threads switch too.

---

# Interview Questions

## What is a context switch?

Switching CPU execution from one process or thread to another.

---

## What is saved during a context switch?

```text
Registers
Program Counter
Stack Pointer
Flags
```

---

## Where is context stored?

```text
PCB (task_struct)
```

---

## Who performs context switching?

```text
Linux Scheduler
```

---

## Why are excessive context switches harmful?

They increase:

```text
CPU Overhead
Cache Misses
Latency
```

---

## Which is cheaper?

```text
Thread Switch
```

because threads share resources.

---

# Quick Revision

```text
Context Switch

Process A Running
        │
        ▼
Save Context
        │
        ▼
Scheduler
        │
        ▼
Load Context
        │
        ▼
Process B Running
```

Context Contains:

✓ Program Counter

✓ Registers

✓ Stack Pointer

✓ Flags

✓ Scheduling State

✓ CPU State

Causes:

✓ Time Slice Expired

✓ Sleep

✓ Interrupt

✓ Higher Priority Process

Tools:

✓ vmstat

✓ pidstat

✓ top

✓ /proc

Remember:

"Multitasking is actually rapid context switching."
