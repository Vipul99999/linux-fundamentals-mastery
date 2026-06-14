# Linux `ps` Command — The Ultimate Guide

> If Processes are the heart of Linux,
>
> then the `ps` command is one of the most important diagnostic tools in the operating system.
>
> Every Linux administrator, DevOps engineer, SRE, backend developer, security engineer, and performance engineer uses `ps`.
>
> When something goes wrong:
>
> ```text
> High CPU
> Memory Leak
> Stuck Service
> Zombie Process
> Unknown Program
> Container Issue
> ```
>
> one of the first commands used is:
>
> ```bash
> ps
> ```

---

# Learning Objectives

After this guide you should understand:

```text
✓ What ps is
✓ How ps works internally
✓ procfs (/proc)
✓ Process inspection
✓ BSD vs UNIX syntax
✓ Process states
✓ Process hierarchy
✓ Thread inspection
✓ Sorting
✓ Filtering
✓ Custom output formatting
✓ Container troubleshooting
✓ Production debugging
✓ Security investigations
✓ Interview questions
```

---

# What is `ps`?

`ps` stands for:

```text
Process Status
```

It displays information about running processes.

Visual:

```text
Linux Kernel
      │
      ▼

 Running Processes

      │
      ▼

     ps

      │
      ▼

Human Readable Output
```

---

# Child-Friendly Explanation

Imagine a school.

Principal wants to know:

```text
Which students are present?
Which class?
Who is absent?
Who is the monitor?
```

The school database contains information.

Similarly:

```text
Linux Kernel
```

contains process information.

`ps` simply displays it.

---

# The Big Picture

```text
Process

 PID
 PPID
 USER
 CPU
 MEMORY
 STATE
 COMMAND

       │

       ▼

      ps
```

---

# How `ps` Works Internally

Many beginners think:

```text
ps talks directly to the kernel
```

Not exactly.

Modern Linux uses:

```text
/proc
```

---

# What is `/proc`?

`/proc` is a virtual filesystem.

Visual:

```text
Kernel Information

      │

      ▼

   /proc

      │

      ▼

   ps Command
```

---

# Example

```bash
ls /proc
```

Output:

```text
1
100
101
102
...
```

These numbers are:

```text
PIDs
```

---

# Inspect a Process

Example:

```bash
ls /proc/1
```

Output:

```text
cmdline
status
stat
fd
maps
environ
...
```

All process information lives here.

---

# How `ps` Gets Information

Visual:

```text
Kernel

   │

   ▼

 /proc/PID

   │

   ▼

   ps
```

`ps` reads files and formats output.

---

# Basic Usage

```bash
ps
```

Example:

```text
 PID TTY      TIME CMD
1000 pts/0 00:00 bash
1010 pts/0 00:00 ps
```

---

# What Does Default ps Show?

Only:

```text
Current Terminal Processes
```

Visual:

```text
Terminal

 ├── bash
 └── ps
```

---

# Why So Few Processes?

Because default behavior is:

```text
Current Session Only
```

---

# Show All Processes

```bash
ps -e
```

or

```bash
ps -A
```

Visual:

```text
Entire System

 ├── systemd
 ├── sshd
 ├── nginx
 ├── docker
 ├── mysql
 └── bash
```

---

# Most Common Command

```bash
ps aux
```

One of the most famous Linux commands.

---

# What Does ps aux Mean?

```text
a = all users

u = user format

x = include processes without terminal
```

---

# Example Output

```text
USER   PID %CPU %MEM COMMAND

root     1  0.0  0.1 systemd
root   500  0.2  0.3 sshd
mysql 1000  1.2  2.1 mysqld
```

---

# Understanding Columns

Let's break down every important field.

---

# USER

Owner of process.

Example:

```text
root

mysql

nginx

ubuntu
```

Visual:

```text
USER

root
```

Means:

```text
Process owned by root
```

---

# PID

Process ID.

Example:

```text
1234
```

Unique identifier.

Visual:

```text
PID

1234
```

---

# PPID

Parent Process ID.

Example:

```text
PID  = 2000

PPID = 1000
```

Visual:

```text
bash
 │
 ▼
python
```

---

# View PPID

```bash
ps -ef
```

Output:

```text
UID PID PPID CMD
```

---

# %CPU

CPU usage.

Example:

```text
75.2
```

Meaning:

```text
Using 75.2% CPU
```

---

# Why Important?

Helps identify:

```text
CPU Hogs
Infinite Loops
Runaway Processes
```

---

# %MEM

Memory usage percentage.

Example:

```text
12.5
```

Meaning:

```text
12.5% of RAM
```

---

# Troubleshooting

High memory usage may indicate:

```text
Memory Leak
Large Cache
Database Buffer Pool
```

---

# VSZ

Virtual Memory Size.

Example:

```text
500000
```

Units:

```text
KB
```

Includes:

```text
Code
Libraries
Mapped Files
Allocated Memory
```

---

# RSS

Resident Set Size.

Very important.

Represents:

```text
Actual Physical RAM
```

Visual:

```text
VSZ = Total Address Space

RSS = Actual RAM Usage
```

---

# Example

```text
VSZ = 2GB

RSS = 500MB
```

Meaning:

```text
Only 500MB currently in RAM
```

---

# TTY

Terminal associated with process.

Example:

```text
pts/0

pts/1

tty1
```

---

# What If TTY Is ?

```text
?
```

Example:

```text
systemd
nginx
docker
```

Means:

```text
No Terminal
```

Usually daemon/service.

---

# TIME

CPU time consumed.

Example:

```text
00:15:32
```

Meaning:

```text
15 minutes CPU time
```

Not wall clock time.

---

# COMMAND

Command that started process.

Example:

```text
nginx

mysqld

python
```

---

# Full Command Line

Use:

```bash
ps -ef
```

Example:

```text
python app.py --port 8080
```

---

# Process States

View:

```bash
ps aux
```

Column:

```text
STAT
```

---

# Common States

| State | Meaning |
|---------|----------|
| R | Running |
| S | Sleeping |
| D | Uninterruptible Sleep |
| T | Stopped |
| Z | Zombie |
| I | Idle Kernel Thread |

---

# Example

```text
R
```

Means:

```text
Running
```

---

# Example

```text
S
```

Means:

```text
Sleeping
```

Most processes spend most time here.

---

# Zombie Detection

```bash
ps aux | grep Z
```

Example:

```text
1234 Z zombie
```

Zombie found.

---

# Process Trees

Processes form hierarchies.

Visual:

```text
systemd
 │
 ├── sshd
 │     └── bash
 │            └── python
 │
 └── nginx
```

---

# Show Process Tree

```bash
ps -ejH
```

or

```bash
ps axjf
```

---

# Visual Example

```text
systemd
 └── sshd
      └── bash
           └── vim
```

---

# Finding Specific Processes

---

## Find nginx

```bash
ps -ef | grep nginx
```

---

## Find Docker

```bash
ps -ef | grep docker
```

---

## Find Python

```bash
ps -ef | grep python
```

---

# Better Alternative

```bash
pgrep nginx
```

---

# Custom Output

One of ps's most powerful features.

---

# -o Option

```bash
ps -eo pid,ppid,user,%cpu,%mem,cmd
```

Output:

```text
PID PPID USER %CPU %MEM CMD
```

---

# Useful Example

```bash
ps -eo pid,user,comm
```

Output:

```text
PID USER COMMAND
```

---

# Sorting Processes

---

## Sort By CPU

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu
```

Top CPU users first.

---

## Sort By Memory

```bash
ps -eo pid,%mem,cmd --sort=-%mem
```

Top memory consumers first.

---

# Top 10 CPU Processes

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu | head
```

---

# Top 10 Memory Processes

```bash
ps -eo pid,%mem,cmd --sort=-%mem | head
```

---

# Viewing Threads

Processes contain threads.

---

# Show Threads

```bash
ps -eLf
```

Output:

```text
PID LWP CMD
```

---

# Important Fields

---

## PID

Process ID

---

## LWP

Light Weight Process (Thread ID)

---

## NLWP

Number of Threads

---

# Example

```bash
ps -o pid,nlwp,cmd -p 1000
```

Output:

```text
PID NLWP CMD
1000  25  java
```

Meaning:

```text
Java process has 25 threads.
```

---

# Security Investigations

Find suspicious processes:

```bash
ps aux
```

Look for:

```text
Unknown Commands
Unexpected Users
High CPU
Strange Network Tools
```

---

# Example

```text
root 9999 crypto_miner
```

Immediate investigation needed.

---

# Container Troubleshooting

Docker containers are processes.

View:

```bash
ps -ef
```

Example:

```text
containerd
dockerd
runc
```

---

# Kubernetes Node Example

```bash
ps -ef | grep kube
```

Output:

```text
kubelet
kube-proxy
```

---

# Performance Troubleshooting Workflow

---

## High CPU

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu
```

---

## High Memory

```bash
ps -eo pid,%mem,cmd --sort=-%mem
```

---

## Zombie Processes

```bash
ps aux | grep Z
```

---

## Parent Relationships

```bash
ps -ef
```

Check:

```text
PPID
```

---

# BSD vs UNIX Syntax

Linux supports both styles.

---

## BSD Style

```bash
ps aux
```

No dash.

---

## UNIX Style

```bash
ps -ef
```

Uses dash.

---

# Common Interview Question

Why do both work?

Because Linux supports:

```text
BSD Compatibility

UNIX Compatibility
```

---

# Useful Real-World Commands

---

## Show Everything

```bash
ps aux
```

---

## Full Format

```bash
ps -ef
```

---

## Process Tree

```bash
ps axjf
```

---

## Show Threads

```bash
ps -eLf
```

---

## Top CPU

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu
```

---

## Top Memory

```bash
ps -eo pid,%mem,cmd --sort=-%mem
```

---

## Show Parent Relationship

```bash
ps -eo pid,ppid,cmd
```

---

## Show User Processes

```bash
ps -u username
```

---

# Common Beginner Mistakes

---

## ❌ PID Never Changes

Wrong.

PIDs are reused.

---

## ❌ VSZ Equals RAM Usage

Wrong.

RSS shows actual RAM usage.

---

## ❌ Sleeping Means Broken

Wrong.

Most processes are sleeping.

---

## ❌ Zombie Uses CPU

Wrong.

Zombie uses almost no resources.

---

## ❌ ps Shows Historical Data

Wrong.

ps shows a snapshot.

---

# Interview Questions

## What does ps stand for?

```text
Process Status
```

---

## How does ps get information?

Through:

```text
/proc filesystem
```

---

## Difference Between VSZ and RSS?

VSZ:

```text
Virtual Memory
```

RSS:

```text
Physical RAM
```

---

## What does STAT Z mean?

```text
Zombie Process
```

---

## How do you view threads?

```bash
ps -eLf
```

---

## How do you show a process tree?

```bash
ps axjf
```

or

```bash
pstree
```

---

## What is PPID?

Parent Process ID.

---

# Visual Summary

```text
Linux Kernel
       │
       ▼

     /proc
       │
       ▼

       ps
       │
       ▼

Process Information
```

Important Fields:

```text
PID
PPID
USER
STAT
%CPU
%MEM
VSZ
RSS
TTY
TIME
CMD
```

Most Important Commands:

```bash
ps aux
ps -ef

ps -eLf

ps axjf

ps -eo pid,%cpu,cmd --sort=-%cpu

ps -eo pid,%mem,cmd --sort=-%mem
```

---

# Final Takeaway

The `ps` command is the foundation of Linux process inspection.

```text
If you cannot see processes,
you cannot troubleshoot Linux.
```

Mastering `ps` enables you to:

```text
Understand Process Hierarchies
Find Resource Problems
Detect Zombies
Investigate Security Issues
Debug Containers
Troubleshoot Production Systems
```

Almost every advanced Linux troubleshooting workflow begins with:

```bash
ps
```
