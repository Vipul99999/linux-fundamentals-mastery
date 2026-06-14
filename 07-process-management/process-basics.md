# `process-basics.md`

````markdown
# Linux Process Basics

# 📖 What is a Process?

A **process** is simply a **running instance of a program**.

Think of it this way:

```text
Program (Stored on Disk)
        ↓ Execute
      Process
````

Example:

```bash
firefox
```

* Firefox installed on disk = Program
* Firefox currently running = Process

---

# 🧠 Child-Friendly Explanation

Imagine a recipe book.

```text
Recipe Book = Program

Cooking Food = Process
```

The recipe book itself does nothing.

Only when someone starts cooking does actual work happen.

Similarly:

```text
Program = Instructions
Process = Instructions Being Executed
```

---

# Why Linux Uses Processes

A computer runs many things simultaneously:

```text
Browser
Music Player
VS Code
Terminal
Docker
Database
System Services
```

Linux uses processes to manage all these tasks safely.

Without processes:

```text
One application crash
      ↓
Entire system crash
```

Processes provide isolation and control.

---

# Visual Overview

```text
                    Linux System

 ┌───────────────┐
 │   Firefox     │
 └───────┬───────┘
         │
      Process

 ┌───────────────┐
 │    VS Code    │
 └───────┬───────┘
         │
      Process

 ┌───────────────┐
 │    Chrome     │
 └───────┬───────┘
         │
      Process

 Linux Kernel manages all processes
```

---

# Program vs Process

| Program                 | Process            |
| ----------------------- | ------------------ |
| Static                  | Dynamic            |
| Stored on disk          | Running in memory  |
| Passive                 | Active             |
| No CPU usage            | Uses CPU           |
| Example: `/usr/bin/vim` | Running Vim editor |

---

# Real Example

Suppose you run:

```bash
vim notes.txt
```

Linux performs:

```text
1. Reads vim binary
2. Loads into RAM
3. Creates Process
4. Assigns PID
5. Schedules CPU
```

Result:

```text
vim process is running
```

---

# What Exists Inside a Process?

Every process contains several parts.

```text
Process
│
├── Program Code
├── Variables
├── Stack
├── Heap
├── Open Files
├── Environment Variables
├── Process ID
└── CPU State
```

Visual:

```text
+------------------+
| Process          |
+------------------+
| Code Segment     |
+------------------+
| Data Segment     |
+------------------+
| Heap             |
+------------------+
| Stack            |
+------------------+
```

---

# Process Memory Layout

```text
High Memory
──────────────────────
Stack
│
│
↓ Grows Down

Free Space

↑ Grows Up
│
Heap

Data Segment

Code Segment
──────────────────────
Low Memory
```

---

# What Makes Every Process Unique?

Linux gives each process:

### 1. PID

Process ID

Example:

```text
PID = 4567
```

No two running processes share the same PID.

---

### 2. Memory Space

Each process gets its own memory.

```text
Process A Memory
────────────────

Process B Memory
────────────────

Process C Memory
────────────────
```

This prevents accidental corruption.

---

### 3. Resources

Processes may own:

```text
Files
Sockets
Network Connections
Pipes
Devices
```

---

# Process Creation

Processes are usually created by other processes.

Visual:

```text
Terminal
   │
   ▼
 Bash
   │
   ├── vim
   ├── python
   ├── gcc
   └── nginx
```

Parent creates child.

---

# Parent and Child Process

Example:

```bash
python app.py
```

Visual:

```text
Terminal
   │
   ▼
 Bash (Parent)
   │
   ▼
 Python (Child)
```

Every process except the first one has a parent.

---

# Process Tree

Linux processes form a tree structure.

```text
systemd (PID 1)
│
├── sshd
│   └── bash
│       └── vim
│
├── nginx
│
├── docker
│
└── mysql
```

View:

```bash
pstree
```

---

# Process IDs (PID)

See current shell PID:

```bash
echo $$
```

Example output:

```text
2456
```

See all processes:

```bash
ps -ef
```

Output:

```text
UID    PID   PPID
root     1      0
root   500      1
user  2456    500
```

---

# Parent Process ID (PPID)

Every process knows its parent.

Example:

```text
PID  = 3000
PPID = 2456
```

Visual:

```text
Parent
 PID=2456
     │
     ▼
Child
 PID=3000
```

---

# Process Owner

Every process belongs to a user.

Example:

```bash
ps -u username
```

Possible owners:

```text
root
ubuntu
john
mysql
nginx
```

This helps Linux enforce permissions.

---

# Process Priority

Some processes are more important.

Example:

```text
Video Player     High Priority
Background Sync  Low Priority
```

Linux scheduler decides CPU allocation.

---

# Foreground Process

Runs directly in terminal.

Example:

```bash
vim file.txt
```

Terminal waits:

```text
Cannot type another command
until vim exits
```

Visual:

```text
Terminal
   │
   ▼
 Foreground Process
```

---

# Background Process

Runs behind the scenes.

Example:

```bash
python app.py &
```

Visual:

```text
Terminal
   │
   ├── User Commands
   │
   └── Background Process
```

---

# Long-Running Processes (Daemons)

Many Linux services run continuously.

Examples:

```text
nginx
sshd
docker
mysql
systemd
```

Visual:

```text
System Boot
      │
      ▼
   Daemon Starts
      │
      ▼
 Runs For Days
```

---

# The First Process in Linux

The first userspace process is:

```text
systemd
```

PID:

```text
1
```

Visual:

```text
Kernel
   │
   ▼
systemd (PID 1)
   │
   ├── sshd
   ├── nginx
   ├── docker
   └── mysql
```

Almost every process eventually traces back to PID 1.

---

# Viewing Running Processes

### Show Current User Processes

```bash
ps
```

### Detailed View

```bash
ps -ef
```

### Real-Time View

```bash
top
```

### Modern View

```bash
htop
```

---

# Process Life Example

```text
User Opens Terminal
        │
        ▼
     Bash Runs
        │
        ▼
 User Executes Python
        │
        ▼
 Process Created
        │
        ▼
 Scheduler Gives CPU
        │
        ▼
 Process Executes
        │
        ▼
 Process Finishes
        │
        ▼
 Process Destroyed
```

---

# Real-World Example: Opening Chrome

```text
Click Chrome
      │
      ▼
Chrome Binary Loaded
      │
      ▼
Process Created
      │
      ▼
PID Assigned
      │
      ▼
Memory Allocated
      │
      ▼
CPU Scheduled
      │
      ▼
Chrome Running
```

---

# Why Processes Matter in Modern Systems

Processes power almost everything:

```text
Web Servers
Cloud Platforms
Containers
Databases
Kubernetes
Docker
AI Systems
CI/CD Pipelines
Monitoring Tools
```

Examples:

```text
Nginx Process
MySQL Process
Docker Process
Redis Process
Kubernetes Components
```

Without processes:

```text
No multitasking
No servers
No cloud computing
No containers
```

---

# Common Beginner Misconceptions

## ❌ Program = Process

Wrong.

```text
Program = File

Process = Running Program
```

---

## ❌ One Program = One Process

Wrong.

Example:

```text
Chrome
├── Tab Process
├── Renderer Process
├── GPU Process
└── Extension Process
```

One application can create many processes.

---

## ❌ Processes Always Run

Wrong.

Processes may be:

```text
Running
Waiting
Sleeping
Stopped
Zombie
```

---

# Interview Questions

### What is a process?

A running instance of a program.

---

### Difference between process and program?

Program is stored on disk.
Process is executing in memory.

---

### What is PID?

Unique Process Identifier.

---

### What is PPID?

Parent Process Identifier.

---

### What is the first process in Linux?

```text
systemd (PID 1)
```

---

### Can one application create multiple processes?

Yes.

Examples:

```text
Chrome
Apache
MySQL
Docker
```

---

# Quick Revision

```text
Program
  ↓ Execute
Process

Every Process Has:
✔ PID
✔ Memory
✔ Resources
✔ Parent
✔ State

Process Types:
✔ Foreground
✔ Background
✔ Daemon

Important Commands:
✔ ps
✔ top
✔ htop
✔ pstree
```

---

# Key Takeaway

A process is the fundamental execution unit of Linux.

```text
Program + Execution + Memory + CPU
               =
            Process
```

Everything running on a Linux system—from a simple text editor to massive cloud infrastructure—ultimately runs as one or more processes managed by the Linux kernel.

```

This file provides the foundation for the entire Process Management section. The next file (`process-lifecycle.md`) should build directly on this and explain exactly **how a process is born, runs, waits, gets scheduled, exits, and gets cleaned up inside the Linux kernel**, with production-grade visuals and modern examples.
```
