# pipes
pipes.md is arguably the chapter where users finally understand the true Unix philosophy.

Before pipes, users think:
```
Command → Output
```
After pipes, users think:

Data Flow Pipeline
```
Data
 │
 ▼
Command 1
 │
 ▼
Command 2
 │
 ▼
Command 3
 │
 ▼
Result

```
This concept powers:
```
Linux Administration
DevOps
Cloud Engineering
SRE
Cybersecurity
Data Engineering
AI/ML Pipelines
CI/CD Systems
Observability
Log Processing
```
---
# Pipes (|) - Connecting Commands Together

# Introduction

Imagine a factory.

```text
Raw Material
     │
     ▼
Machine 1
     │
     ▼
Machine 2
     │
     ▼
Machine 3
     │
     ▼
Finished Product
```

Linux commands can work exactly the same way.

Instead of:

```text
Machine
```

we have:

```text
Command
```

Instead of:

```text
Physical Product
```

we have:

```text
Data
```

This connection is called:

```text
Pipe
```

represented by:

```bash
|
```

---

# Why Pipes Exist

Without pipes:

```bash
ls
```

Produces:

```text
file1.txt
file2.txt
file3.txt
```

But what if we want:

```text
Count Files

Sort Files

Filter Files

Analyze Files
```

Without pipes:

```text
Command
 ↓
Copy Output
 ↓
Run Another Command
 ↓
Paste Input
```

Very inefficient.

Pipes automate this.

---

# What Is A Pipe?

A pipe connects:

```text
stdout
```

of one command to:

```text
stdin
```

of another command.

Visual:

```text
stdout
   │
   ▼
Command 1
   │
   ▼
    |
   │
   ▼
Command 2
   │
   ▼
stdin
```

---

# Most Important Definition

```text
Pipe
=
Output Of One Command
Becomes
Input Of Another Command
```

---

# Internal Architecture

Without Pipe:

```text
Command
   │
   ▼
Screen
```

With Pipe:

```text
Command 1
     │
     ▼
Command 2
     │
     ▼
Screen
```

---

# First Example

```bash
ls | wc -l
```

Meaning:

```text
List Files
      │
      ▼
Count Lines
```

Visual:

```text
ls
 │
 ▼

file1.txt
file2.txt
file3.txt

 │
 ▼

wc -l

 │
 ▼

3
```

---

# Understanding Data Flow

Step 1

```bash
ls
```

Output:

```text
a.txt
b.txt
c.txt
```

Step 2

Pipe sends output:

```text
a.txt
b.txt
c.txt
```

to:

```bash
wc -l
```

Step 3

Result:

```text
3
```

---

# Visual Pipeline

```text
Files
 │
 ▼

ls
 │
 ▼

a.txt
b.txt
c.txt

 │
 ▼

wc -l
 │
 ▼

3
```

---

# Why Pipes Are Powerful

Without pipes:

```text
Save Output To File
Open File
Run Another Command
```

With pipes:

```text
Direct Command Communication
```

---

# Real World Analogy

Imagine water pipes.

```text
Tank A
  │
  ▼
Pipe
  │
  ▼
Tank B
```

Linux pipes move:

```text
Data
```

instead of:

```text
Water
```

---

# Pipe Symbol

```bash
|
```

Located above Enter key on most keyboards.

---

# Common Pipe Commands

```bash
ls | wc -l

cat file.txt | sort

ps aux | grep nginx

df -h | grep nvme

journalctl | less
```

---

# Filtering Data With grep

One of the most common uses.

Example:

```bash
ps aux | grep nginx
```

Visual:

```text
Running Processes
        │
        ▼
grep nginx
        │
        ▼
Only nginx Processes
```

---

# Process Investigation

```bash
ps aux | grep node
```

Visual:

```text
All Processes
      │
      ▼
Filter Node.js
      │
      ▼
Node Processes
```

Used daily by DevOps engineers.

---

# Log Analysis

```bash
cat app.log | grep ERROR
```

Visual:

```text
Application Log
       │
       ▼
Filter Errors
       │
       ▼
Only Error Lines
```

---

# Modern Alternative

Often:

```bash
grep ERROR app.log
```

is better than:

```bash
cat app.log | grep ERROR
```

But the pipe teaches the concept.

---

# Sorting Data

Example:

```bash
cat names.txt | sort
```

Visual:

```text
Input

Charlie
Alice
Bob

   │
   ▼

sort

   │
   ▼

Alice
Bob
Charlie
```

---

# Remove Duplicates

Example:

```bash
cat names.txt | sort | uniq
```

Visual:

```text
names.txt
     │
     ▼

sort
     │
     ▼

uniq
     │
     ▼

Unique Names
```

---

# Multi-Stage Pipelines

Pipelines can be long.

Example:

```bash
cat access.log | grep ERROR | sort | uniq
```

Visual:

```text
Log File
   │
   ▼

grep ERROR
   │
   ▼

sort
   │
   ▼

uniq
   │
   ▼

Result
```

---

# Pipeline Thinking

Beginners think:

```text
Command
```

Experts think:

```text
Data Pipeline
```

Visual:

```text
Input
 │
 ▼
Filter
 │
 ▼
Transform
 │
 ▼
Aggregate
 │
 ▼
Output
```

---

# Count Matching Lines

Example:

```bash
grep ERROR app.log | wc -l
```

Visual:

```text
Log File
    │
    ▼

ERROR Lines
    │
    ▼

Count
```

Result:

```text
Number Of Errors
```

---

# Monitoring Example

Check memory:

```bash
free -h | grep Mem
```

Visual:

```text
Memory Stats
      │
      ▼
Filter Mem Line
```

---

# Disk Monitoring

```bash
df -h | grep "/"
```

Visual:

```text
Disk Usage
      │
      ▼
Filter Filesystems
```

---

# Modern DevOps Examples

---

# Docker Containers

```bash
docker ps | grep nginx
```

Visual:

```text
All Containers
      │
      ▼
nginx Containers
```

---

# Kubernetes Pods

```bash
kubectl get pods | grep CrashLoopBackOff
```

Visual:

```text
Pods
 │
 ▼
Filter Failed Pods
```

---

# CI/CD Logs

```bash
cat build.log | grep FAILED
```

Visual:

```text
Build Log
     │
     ▼
Failed Steps
```

---

# Security Examples

---

# Failed Login Attempts

```bash
cat auth.log | grep Failed
```

Visual:

```text
Authentication Log
        │
        ▼
Failed Attempts
```

---

# SSH Activity

```bash
cat auth.log | grep ssh
```

Visual:

```text
Auth Log
    │
    ▼
SSH Events
```

---

# AI / Data Science Examples

---

# Dataset Analysis

```bash
cat dataset.csv | wc -l
```

Visual:

```text
Dataset
   │
   ▼
Count Rows
```

---

# Count Records

```bash
cat users.csv | tail -n +2 | wc -l
```

Visual:

```text
CSV
 │
 ▼
Skip Header
 │
 ▼
Count Records
```

---

# Pipe vs Redirection

Redirection:

```bash
ls > files.txt
```

Visual:

```text
Command
   │
   ▼
File
```

Pipe:

```bash
ls | wc -l
```

Visual:

```text
Command
   │
   ▼
Command
```

---

# Comparison

| Feature           | Pipe | Redirection |
| ----------------- | ---- | ----------- |
| Command → Command | Yes  | No          |
| Command → File    | No   | Yes         |
| Stream Data       | Yes  | No          |
| Build Pipelines   | Yes  | No          |

---

# stderr And Pipes

Important:

Pipes only send:

```text
stdout
```

not:

```text
stderr
```

Visual:

```text
Command
 │
 ├── stdout ──► Pipe
 │
 └── stderr ──► Terminal
```

---

# Combining stderr

Example:

```bash
command 2>&1 | grep error
```

Visual:

```text
stdout
   │
   ├──► Pipe
   │
stderr
   │
   └──► Pipe
```

---

# Modern Logging Pipelines

```bash
journalctl | grep ERROR | less
```

Visual:

```text
System Logs
     │
     ▼
Filter Errors
     │
     ▼
Scrollable View
```

---

# Pipe Chains

Example:

```bash
ps aux | grep node | wc -l
```

Visual:

```text
Processes
    │
    ▼

grep node
    │
    ▼

Count
    │
    ▼

Result
```

---

# Internal Kernel View

```text
stdout Buffer
       │
       ▼
Kernel Pipe
       │
       ▼
stdin Buffer
```

The kernel manages the pipe.

---

# Common Mistakes

## Thinking Pipes Use Files

Wrong.

Pipe uses memory streams.

Visual:

```text
Command A
     │
     ▼
Memory
     │
     ▼
Command B
```

No file created.

---

## Using Files Unnecessarily

Bad:

```bash
ls > temp.txt

wc -l temp.txt
```

Better:

```bash
ls | wc -l
```

---

## Forgetting stderr

```bash
command | grep error
```

Only stdout flows through pipe.

---

# Memory Trick

Think:

```text
>

Command → File

|

Command → Command
```

---

# Visual Cheat Sheet

```text
Pipe
│
├── Command → Command
│
├── stdout → stdin
│
├── Real-Time Data Flow
│
├── No Temporary File
│
└── Pipeline Creation
```

---

# Real Linux Philosophy

Unix philosophy:

```text
Do One Thing Well
```

Example:

```text
grep
 │
 ▼
Filter

sort
 │
 ▼
Sort

uniq
 │
 ▼
Remove Duplicates

wc
 │
 ▼
Count
```

Pipe combines them.

---

# Hands-On Lab

Create:

```bash
echo -e "Alice\nBob\nCharlie" > names.txt
```

Count names:

```bash
cat names.txt | wc -l
```

Sort names:

```bash
cat names.txt | sort
```

Find Bob:

```bash
cat names.txt | grep Bob
```

Chain commands:

```bash
cat names.txt | grep Bob | wc -l
```

Observe how data flows.

---

# Interview Questions

### What is a pipe?

A mechanism that sends stdout of one command to stdin of another command.

---

### What symbol represents a pipe?

```bash
|
```

---

### Does a pipe create a file?

No.

It uses in-memory data streams.

---

### What does this do?

```bash
ls | wc -l
```

Counts items produced by ls.

---

### Do pipes transfer stderr?

No.

Only stdout unless stderr is redirected.

---

### Difference between > and | ?

```text
>

Command → File

|

Command → Command
```

---

# Quick Cheat Sheet

```bash
ls | wc -l

ps aux | grep nginx

cat file.txt | sort

cat file.txt | sort | uniq

grep ERROR app.log | wc -l

journalctl | grep ERROR

command 2>&1 | grep error
```

# Key Takeaway

```text
Redirection
│
└── Command → File

Pipe
│
└── Command → Command

Modern Linux
│
└── Data Pipelines Everywhere
```

```text
Input
 │
 ▼
Command
 │
 ▼
Command
 │
 ▼
Command
 │
 ▼
Result
```

