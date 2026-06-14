# Linux Process Control Block (PCB)

> If a process is a living person, then the Process Control Block (PCB) is the person's complete identity card, medical record, work history, address, permissions, and current status—all stored in one place.
>
> The Linux kernel uses the PCB to manage every process in the system.

---

# Why Do We Need a PCB?

Imagine Linux is running:

```text
Chrome
VS Code
Docker
MySQL
Redis
Nginx
Python
Java
```

Linux must remember:

```text
Who owns this process?
What is its PID?
What files are open?
Which memory belongs to it?
What is its current state?
Which CPU was it using?
```

Where is this information stored?

```text
Process Control Block (PCB)
```

---

# Child-Friendly Explanation

Imagine a school.

Every student has:

```text
Student ID
Name
Class
Attendance
Marks
Address
```

School stores this information in a record.

Similarly:

```text
Process
   ↓
PCB
```

Linux stores information about every process inside a PCB.

---

# What is a PCB?

A PCB is a kernel data structure that stores all information required to manage a process.

Visual:

```text
+----------------------------------+
| Process Control Block (PCB)      |
+----------------------------------+
| PID                              |
| State                            |
| CPU Registers                    |
| Memory Information               |
| Scheduling Information           |
| Open Files                       |
| Signals                          |
| Security Credentials             |
| Parent Information               |
+----------------------------------+
```

---

# PCB in Linux

In Linux, the PCB is represented by:

```c
struct task_struct
```

This is one of the most important structures in the Linux kernel.

Visual:

```text
Process
   │
   ▼
task_struct
```

Every process has its own `task_struct`.

---

# Big Picture

```text
Linux Kernel

 ┌─────────────────────┐
 │ Process A PCB       │
 └─────────────────────┘

 ┌─────────────────────┐
 │ Process B PCB       │
 └─────────────────────┘

 ┌─────────────────────┐
 │ Process C PCB       │
 └─────────────────────┘
```

The kernel tracks all processes through their PCBs.

---

# Process Creation and PCB

When a process starts:

```text
fork()
   ↓
Kernel Creates PCB
   ↓
Assign PID
   ↓
Allocate Memory
   ↓
Scheduler Tracks Process
```

Visual:

```text
User Starts Program
         │
         ▼
Process Created
         │
         ▼
PCB Created
```

No PCB means:

```text
No Process Management
```

---

# Main Components of a PCB

```text
PCB
│
├── Process ID
├── Process State
├── CPU Context
├── Scheduling Data
├── Memory Data
├── File Data
├── Signals
├── Security Data
└── Parent/Child Links
```

Let's explore each part.

---

# 1. Process ID (PID)

Every process has a unique identifier.

Example:

```text
PID = 4521
```

Visual:

```text
PCB
│
└── PID = 4521
```

Linux uses PID to identify the process.

Commands:

```bash
ps
```

```bash
top
```

```bash
kill 4521
```

All rely on PID.

---

# Why PID Matters

Without PID:

```text
Kernel cannot distinguish processes
```

Imagine:

```text
100 Chrome Processes
```

PID identifies each one uniquely.

---

# 2. Process State

Kernel stores current process state.

Example:

```text
Running
Sleeping
Zombie
Stopped
```

Visual:

```text
PCB
│
└── State = Running
```

The scheduler checks this field constantly.

---

# Example

```bash
ps aux
```

Output:

```text
R
S
D
Z
```

These values come from the PCB.

---

# 3. CPU Context

This is one of the most important PCB fields.

It stores:

```text
CPU Registers
Program Counter
Stack Pointer
Flags
```

Visual:

```text
PCB
│
├── Registers
├── PC
├── SP
└── Flags
```

This information allows Linux to pause and resume processes.

---

# Child-Friendly Example

Imagine playing a game.

You save:

```text
Level
Position
Health
Inventory
```

Later:

```text
Load Save
Continue Playing
```

CPU context works similarly.

---

# Program Counter (PC)

Stores:

```text
Next instruction to execute
```

Example:

```text
Instruction #500
```

Visual:

```text
Code

100
101
102
103
...
500 ← Current Position
501
502
```

Without PC:

```text
Process cannot resume execution
```

---

# Stack Pointer (SP)

Points to current stack location.

Visual:

```text
Stack

+--------+
| Data   |
+--------+
| Data   |
+--------+
| SP     |
+--------+
```

Needed for:

```text
Functions
Local Variables
Function Calls
```

---

# CPU Registers

Registers hold temporary values.

Examples:

```text
AX
BX
CX
DX
RAX
RBX
```

Visual:

```text
CPU
│
├── Register A
├── Register B
├── Register C
└── Register D
```

PCB stores register values during context switches.

---

# 4. Scheduling Information

Scheduler uses PCB to decide:

```text
Who runs next?
```

Stored information:

```text
Priority
CPU Usage
Scheduling Policy
Time Slice
```

Visual:

```text
PCB
│
├── Priority
├── Nice Value
├── Runtime
└── Scheduler Data
```

---

# Example

High-priority process:

```text
Database
```

Low-priority process:

```text
Background Sync
```

Scheduler uses PCB data to make decisions.

---

# 5. Memory Information

PCB contains memory-related information.

Visual:

```text
PCB
│
└── Memory Information
```

---

# Memory Layout

```text
+-------------------+
| Stack             |
+-------------------+
| Heap              |
+-------------------+
| Data Segment      |
+-------------------+
| Code Segment      |
+-------------------+
```

PCB stores references to these regions.

---

# Why Memory Information Is Needed

Kernel must know:

```text
Which memory belongs to process?
```

Without this:

```text
Processes could overwrite each other
```

System would crash frequently.

---

# 6. Open File Information

Processes often use files.

Examples:

```text
Logs
Databases
Configuration Files
Sockets
Pipes
```

PCB stores references.

Visual:

```text
PCB
│
├── File Descriptor 0
├── File Descriptor 1
├── File Descriptor 2
└── Other Open Files
```

---

# Standard File Descriptors

Every process starts with:

```text
0 → stdin
1 → stdout
2 → stderr
```

Visual:

```text
Keyboard
   │
stdin (0)

stdout (1)
   │
Terminal

stderr (2)
   │
Terminal
```

---

# Example

See open files:

```bash
lsof -p PID
```

These entries are tracked via PCB structures.

---

# 7. Signal Information

Processes can receive signals.

Examples:

```text
SIGTERM
SIGKILL
SIGSTOP
SIGINT
```

PCB stores signal-related data.

Visual:

```text
PCB
│
├── Pending Signals
├── Signal Handlers
└── Signal Masks
```

---

# Example

Press:

```text
CTRL + C
```

Signal sent:

```text
SIGINT
```

Kernel checks PCB signal information.

---

# 8. Security Credentials

Linux must know:

```text
Who owns this process?
```

Stored information:

```text
UID
GID
Capabilities
Permissions
```

Visual:

```text
PCB
│
├── User ID
├── Group ID
└── Capabilities
```

---

# Example

```bash
nginx
```

May run as:

```text
www-data
```

PCB stores ownership information.

---

# 9. Parent and Child Relationships

Linux processes form a tree.

Visual:

```text
systemd
   │
   └── bash
          │
          └── python
```

PCB stores:

```text
Parent Process
Child Processes
Sibling Processes
```

---

# Why Store Parent Information?

Needed for:

```text
wait()
Zombie Cleanup
Signals
Process Trees
```

---

# Complete PCB Visualization

```text
+--------------------------------+
| Process Control Block          |
+--------------------------------+
| PID                            |
| PPID                           |
| State                          |
| Priority                       |
| CPU Registers                  |
| Program Counter                |
| Stack Pointer                  |
| Memory Information             |
| Open Files                     |
| Signals                        |
| User Credentials               |
| Scheduling Information         |
| Parent/Child Links             |
+--------------------------------+
```

---

# PCB and Context Switching

This is where PCB becomes critical.

Imagine:

```text
Process A Running
```

CPU switches to:

```text
Process B
```

Kernel must save:

```text
Registers
Program Counter
Stack Pointer
```

Where?

```text
PCB of Process A
```

Visual:

```text
Process A Running
         │
         ▼
Save State To PCB
         │
         ▼
Run Process B
```

Later:

```text
Restore State From PCB
```

Process A continues exactly where it stopped.

---

# Modern Linux Example

Chrome:

```text
Tab 1
Tab 2
Tab 3
GPU Process
Network Process
```

Each process:

```text
Own PCB
Own PID
Own Memory
Own Files
```

The kernel tracks thousands of PCBs simultaneously.

---

# Containers and PCB

Docker containers are not special processes.

Inside Linux:

```text
Container Process
      │
      ▼
PCB
```

Container processes still require:

```text
PID
Memory
Files
Scheduling
Signals
```

All stored in PCB structures.

---

# Kubernetes Example

Pod:

```text
Nginx Container
```

Inside:

```text
Linux Process
```

Inside kernel:

```text
task_struct
```

Kubernetes ultimately relies on Linux PCBs.

---

# What Happens When Process Exits?

Process ends:

```text
exit()
```

Kernel:

```text
Cleanup Files
Cleanup Memory
Remove Scheduling Data
Remove PCB
```

Visual:

```text
Running
   │
Exit
   │
PCB Destroyed
```

---

# Common Beginner Mistakes

## ❌ PCB is the Process

Wrong.

```text
Process = Running Entity

PCB = Information About Process
```

---

## ❌ PCB Exists in User Space

Wrong.

PCB lives in:

```text
Kernel Space
```

---

## ❌ PCB Stores Program Code

Not directly.

PCB stores references and management information.

---

# PCB vs Process

| Process | PCB |
|----------|------|
| Executes code | Stores metadata |
| Uses CPU | Used by kernel |
| Lives in memory | Lives in kernel |
| Runs program | Manages process |

---

# Interview Questions

## What is PCB?

A kernel data structure containing all information required to manage a process.

---

## What is PCB called in Linux?

```c
task_struct
```

---

## Why is PCB important?

It allows the kernel to:

```text
Track
Schedule
Pause
Resume
Terminate
```

processes.

---

## Which PCB fields are required for context switching?

```text
Registers
Program Counter
Stack Pointer
CPU State
```

---

## Where is PCB stored?

```text
Kernel Space
```

---

## Does every process have a PCB?

```text
Yes
```

Every Linux process has its own PCB.

---

# Quick Revision

```text
Process Control Block (PCB)

Stores:

✓ PID
✓ PPID
✓ State
✓ CPU Registers
✓ Program Counter
✓ Stack Pointer
✓ Memory Information
✓ Scheduling Information
✓ Open Files
✓ Signals
✓ User Credentials
✓ Parent/Child Links
```

Remember:

```text
Process Runs

PCB Remembers Everything
```

Without the PCB, Linux would have no way to manage, schedule, pause, resume, or terminate processes.
