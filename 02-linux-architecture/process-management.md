# Process Management

Process Management is one of the most important responsibilities of the Linux kernel. It is responsible for creating, scheduling, executing, monitoring, and terminating processes while efficiently sharing CPU resources among thousands of running tasks.

---

# Table of Contents

1. What is a Process?
2. Why Process Management Exists
3. Process Architecture
4. Program vs Process
5. Process Lifecycle
6. Process States
7. Process Control Block (PCB)
8. Process Creation
9. Process Scheduling
10. Context Switching
11. Threads vs Processes
12. Process Communication
13. Process Synchronization
14. Signals
15. Process Hierarchy
16. Zombie Processes
17. Orphan Processes
18. Real-World Examples
19. Linux Commands
20. Interview Questions

---

# What is a Process?

A Process is an executing instance of a program.

Example:

```text
Program:
Google Chrome

Process:
Running Chrome Instance
```

Examples:

```text
bash
chrome
nginx
mysqld
python
node
```

---

# Why Process Management Exists

Modern systems run multiple applications simultaneously.

```text
Chrome
VS Code
Spotify
Docker
Terminal
MySQL
```

The kernel must:

* Allocate CPU
* Manage memory
* Schedule execution
* Handle communication
* Ensure fairness

---

# Process Architecture

```text
+----------------------+
|       Process        |
+----------------------+
| Program Code         |
| Data Segment         |
| Heap                 |
| Stack                |
| Open Files           |
| Registers            |
| Process ID (PID)     |
+----------------------+
```

---

# Program vs Process

| Program             | Process          |
| ------------------- | ---------------- |
| Static File         | Running Instance |
| Stored on Disk      | Stored in Memory |
| Passive             | Active           |
| Example: chrome.exe | Running Chrome   |

Visualization:

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

# Process Lifecycle

```text
      New
       │
       ▼
      Ready
       │
       ▼
     Running
     /     \
    /       \
Waiting    Ready
    │
    ▼
Running
    │
    ▼
Terminated
```

---

# Process States

## New

Process is being created.

```text
fork()
```

---

## Ready

Waiting for CPU allocation.

```text
Ready Queue
     │
     ▼
Scheduler
```

---

## Running

Currently executing.

```text
CPU
 │
 ▼
Process Running
```

---

## Waiting / Blocked

Waiting for:

* Disk I/O
* Network
* User Input

```text
Running
   │
   ▼
Disk Read Request
   │
   ▼
Waiting State
```

---

## Terminated

Execution finished.

```text
exit()
```

---

# Process Control Block (PCB)

Kernel stores process information in PCB.

```text
+----------------------+
| Process Control Block|
+----------------------+
| PID                  |
| State                |
| CPU Registers        |
| Program Counter      |
| Priority             |
| Memory Info          |
| Open Files           |
+----------------------+
```

---

# Process Identification

Every process has a unique PID.

Example:

```bash
ps aux
```

Output:

```text
PID    COMMAND
1001   bash
1045   nginx
1090   python
```

---

# Process Creation

Linux uses:

```c
fork()
```

Visualization:

```text
Parent Process
        │
        ▼
      fork()
        │
        ▼
 ┌───────────────┐
 │               │
 ▼               ▼
Parent         Child
```

---

# fork() Example

```c
pid_t pid = fork();
```

Result:

```text
Parent Process
      │
      ├── Parent Continues
      │
      └── Child Created
```

---

# exec()

Loads a new program.

```c
execvp("ls", args);
```

Visualization:

```text
Old Process Image
        │
        ▼
      exec()
        │
        ▼
New Program Loaded
```

---

# Process Scheduling

Scheduler decides:

```text
Who runs?
When runs?
How long runs?
```

---

# Scheduling Architecture

```text
Ready Queue
      │
      ▼
Linux Scheduler
      │
      ▼
CPU
      │
      ▼
Running Process
```

---

# CPU Scheduling

```text
Process A
Process B
Process C
Process D

      │
      ▼

Scheduler

      │
      ▼

CPU
```

---

# Time Sharing

Linux rapidly switches processes.

```text
Time →
------------------------------------------------

Process A : ███

Process B :    ███

Process C :       ███

Process D :          ███
```

Appears simultaneous.

---

# Context Switching

CPU switches between processes.

```text
Process A Running
        │
        ▼
Save State
        │
        ▼
Load Process B State
        │
        ▼
Process B Running
```

---

# Context Switch Components

Saved Information:

```text
Program Counter
Registers
Stack Pointer
CPU State
Memory Mapping
```

---

# Process Hierarchy

Linux processes form a tree.

```text
init/systemd (PID 1)
        │
 ┌──────┼──────┐
 │      │      │
 ▼      ▼      ▼
sshd   nginx  cron
 │
 ▼
bash
 │
 ▼
python
```

---

# Parent and Child Processes

```text
Parent
  │
  ├── Child 1
  │
  ├── Child 2
  │
  └── Child 3
```

---

# Threads vs Processes

| Feature       | Process  | Thread |
| ------------- | -------- | ------ |
| Memory        | Separate | Shared |
| Creation Cost | High     | Low    |
| Communication | Slower   | Faster |
| Isolation     | Strong   | Weak   |

---

# Process with Threads

```text
+-----------------------+
|       Process         |
+-----------------------+
| Thread 1              |
| Thread 2              |
| Thread 3              |
+-----------------------+
```

---

# Inter Process Communication (IPC)

Processes communicate using:

```text
IPC
│
├── Pipes
├── Message Queues
├── Shared Memory
├── Sockets
├── Signals
└── Semaphores
```

---

# Pipe Communication

```text
Process A
     │
     ▼
    Pipe
     │
     ▼
Process B
```

Example:

```bash
ls | grep txt
```

---

# Shared Memory

Fastest IPC mechanism.

```text
Process A
      │
      ▼
Shared Memory
      ▲
      │
Process B
```

---

# Signals

Signals notify processes.

Examples:

| Signal  | Meaning          |
| ------- | ---------------- |
| SIGINT  | Ctrl+C           |
| SIGKILL | Force Kill       |
| SIGSTOP | Stop Process     |
| SIGCONT | Continue Process |

Visualization:

```text
User
 │
 ▼
Signal
 │
 ▼
Process
```

---

# Zombie Processes

Zombie = Finished process whose parent hasn't collected status.

```text
Child Finished
      │
      ▼
Zombie
      │
      ▼
Parent Reads Status
      │
      ▼
Removed
```

---

# Orphan Processes

Parent terminates first.

```text
Parent Dies
      │
      ▼
Child Survives
      │
      ▼
Adopted by systemd
```

---

# Real-World Example

Running:

```bash
firefox
```

Flow:

```text
Shell
 │
 ▼
fork()
 │
 ▼
Child Process
 │
 ▼
exec()
 │
 ▼
Firefox Loaded
 │
 ▼
Running Process
```

---

# Linux Process Commands

## View Processes

```bash
ps aux
```

---

## Real-Time Monitoring

```bash
top
```

---

## Enhanced Monitoring

```bash
htop
```

---

## Kill Process

```bash
kill PID
```

---

## Process Tree

```bash
pstree
```

Visualization:

```text
systemd
 ├── sshd
 ├── nginx
 ├── mysql
 └── docker
```

---

# Production Use Cases

Process management powers:

```text
Cloud Servers
Docker Containers
Kubernetes Pods
Android
Databases
Web Servers
AI Workloads
Operating Systems
```

---

# Interview Questions

## Beginner

1. What is a process?
2. Difference between program and process?
3. What is PID?
4. What is a PCB?
5. What are process states?

## Intermediate

6. What does fork() do?
7. What does exec() do?
8. Explain context switching.
9. Explain process scheduling.
10. What is IPC?

## Advanced

11. Explain Linux CFS Scheduler.
12. What causes zombie processes?
13. Explain orphan processes.
14. How does shared memory work?
15. Explain thread scheduling.

---

# Summary

Process Management is responsible for creating, scheduling, executing, synchronizing, and terminating processes. Through scheduling, context switching, IPC, signals, and process hierarchies, Linux efficiently manages thousands of concurrent tasks while ensuring stability, fairness, and performance.
