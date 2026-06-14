# Linux nice & renice — Process Priorities Explained Visually

> Not all processes are equally important.
>
> Consider a production server:
>
> ```text
> PostgreSQL Database
> Nginx Web Server
> Backup Script
> Log Compression
> AI Training Job
> ```
>
> If CPU becomes busy:
>
> ```text
> Who should get CPU first?
> ```
>
> Linux answers this using:
>
> ```text
> Priorities
> Nice Values
> Scheduler Weights
> ```
>
> and the commands:
>
> ```bash
> nice
> renice
> ```
>
> Understanding these concepts is essential for:
>
> - Linux Administration
> - DevOps
> - SRE
> - Performance Engineering
> - Cloud Computing
> - Backend Systems
> - Production Troubleshooting

---

# Learning Objectives

After completing this guide, you should understand:

```text
✓ Process Priority
✓ Nice Values
✓ Scheduler Weights
✓ nice Command
✓ renice Command
✓ CFS Scheduling
✓ CPU Fairness
✓ CPU Starvation
✓ Production Tuning
✓ Real-world Examples
✓ Docker & Kubernetes Considerations
✓ Interview Questions
```

---

# Child-Friendly Explanation

Imagine a teacher helping students.

Students:

```text
Student A
Student B
Student C
Student D
```

All need help.

Teacher decides:

```text
Who should get attention first?
```

Maybe:

```text
Exam Tomorrow → Higher Priority

Homework Due Next Week → Lower Priority
```

Linux scheduler works similarly.

---

# What is Process Priority?

Priority determines:

```text
How much CPU time
a process should receive
relative to others.
```

Visual:

```text
CPU

      ▲

      │

Scheduler

      ▲

      │

Processes
```

The scheduler uses priorities to make decisions.

---

# Why Priorities Exist

Imagine:

```text
Database Server

Backup Script

Video Encoder
```

CPU becomes busy.

Should backup script delay database requests?

```text
No
```

Priority helps Linux choose.

---

# Important Truth

Priority does NOT mean:

```text
Process Always Runs First
```

It means:

```text
Process Gets More Scheduling Preference
```

---

# Linux Priority System

Linux uses:

```text
Nice Values
```

to influence priorities.

---

# Why "Nice"?

Historical UNIX term.

Meaning:

```text
How nice a process is
toward other processes.
```

---

# Child-Friendly Explanation

Imagine:

Student A says:

```text
I will wait.
```

Very nice.

Student B says:

```text
I need help first.
```

Less nice.

Linux uses the same idea.

---

# Nice Value Range

```text
-20 → Highest Priority

  0 → Default

 19 → Lowest Priority
```

Visual:

```text
High Priority

 -20

 -10

  0

 10

 19

Low Priority
```

---

# Important Rule

Lower number:

```text
Higher Priority
```

Higher number:

```text
Lower Priority
```

Many beginners get this backward.

---

# Quick Memory Trick

```text
Nice Process

Waits More

Gets Less CPU
```

Therefore:

```text
Higher Nice Value
=
Lower Priority
```

---

# Visual Example

```text
Process A = Nice 0

Process B = Nice 10
```

Result:

```text
Process A gets more CPU
```

---

# Default Nice Value

Most programs start with:

```text
0
```

Example:

```bash
ps -eo pid,ni,cmd
```

Output:

```text
PID NI CMD

1234 0 nginx

2345 0 python
```

---

# Viewing Nice Values

Use:

```bash
ps -eo pid,ni,cmd
```

Example:

```text
PID NI COMMAND

1234  0 nginx

5678 10 backup.sh
```

---

# Understanding CFS

Modern Linux uses:

```text
CFS
```

(Completely Fair Scheduler)

---

# CFS Does NOT Use Nice Directly

Instead:

```text
Nice Value
      │
      ▼
Weight
      │
      ▼
CPU Share
```

---

# Why Weights?

Scheduler works with:

```text
Weights
```

instead of raw nice values.

---

# Simplified Weight Example

```text
Nice 0  → Weight 1024

Nice 5  → Weight 335

Nice 10 → Weight 110
```

---

# Visual

```text
Weight

1024 ███████████████

 335 █████

 110 ██
```

More weight:

```text
More CPU Time
```

---

# CPU Sharing Example

Two processes:

```text
Process A Weight = 1024

Process B Weight = 1024
```

Result:

```text
50%

50%
```

CPU split evenly.

---

# Another Example

```text
Process A Weight = 1024

Process B Weight = 335
```

Result:

```text
A receives much more CPU
```

---

# The nice Command

Used when starting a process.

Syntax:

```bash
nice [OPTION] COMMAND
```

---

# Example

```bash
nice -n 10 backup.sh
```

Meaning:

```text
Start backup.sh

Nice Value = 10
```

Lower scheduling priority.

---

# Visual

```text
Backup Job

Nice 10

Less CPU Pressure
```

---

# Check Result

```bash
ps -eo pid,ni,cmd
```

Output:

```text
PID NI CMD

4000 10 backup.sh
```

---

# High-Priority Process

Example:

```bash
sudo nice -n -10 database_process
```

Meaning:

```text
Higher Scheduling Priority
```

---

# Why sudo?

Normal users cannot set:

```text
Negative Nice Values
```

because it could affect system fairness.

---

# The renice Command

Changes priority of an existing process.

Syntax:

```bash
renice NICE_VALUE PID
```

---

# Example

Current:

```text
PID 1234

Nice = 0
```

Change:

```bash
renice 10 -p 1234
```

Result:

```text
Nice = 10
```

---

# Visual

```text
Running Process

     │

renice

     ▼

New Priority
```

---

# Renice Multiple Processes

```bash
renice 5 -p 1000 1001 1002
```

All updated.

---

# Renice by User

Example:

```bash
renice 10 -u mysql
```

Changes:

```text
All mysql user processes
```

---

# Viewing Priority Information

```bash
top
```

Columns:

```text
PR

NI
```

---

# Example

```text
PR NI

20  0
```

---

# What is PR?

Kernel priority.

---

# What is NI?

Nice value.

---

# Relationship

```text
Nice Value
      │
      ▼
Priority
      │
      ▼
Scheduling Weight
```

---

# Production Example

Server:

```text
Database

Backup Job
```

Database:

```text
Nice = -5
```

Backup:

```text
Nice = 10
```

Result:

```text
Database remains responsive.
```

---

# Video Encoding Example

Bad:

```bash
ffmpeg huge_video.mp4
```

Consumes CPU aggressively.

Better:

```bash
nice -n 15 ffmpeg huge_video.mp4
```

Background-friendly.

---

# Log Compression Example

```bash
nice -n 19 gzip huge.log
```

Lowest priority.

Perfect for maintenance tasks.

---

# Build Servers

Example:

```bash
nice -n 10 make -j8
```

Compilation won't overwhelm production services.

---

# AI / ML Example

Training:

```text
CPU Intensive
```

Run:

```bash
nice -n 15 python train.py
```

to reduce impact.

---

# CPU Starvation

Can priorities cause starvation?

Historically:

```text
Yes
```

Modern CFS:

```text
Much Less Likely
```

because fairness is built into scheduler.

---

# Nice Values vs Real-Time Scheduling

Very important.

Nice affects:

```text
Normal Scheduling
```

Only.

---

# Nice Does NOT Affect

```text
SCHED_FIFO

SCHED_RR

SCHED_DEADLINE
```

---

# Visual

```text
Normal Process

Nice Value Used
```

```text
Real-Time Process

Nice Ignored
```

---

# Docker and Nice Values

Containers use normal Linux scheduling.

Example:

```bash
docker run ...
```

Container processes have:

```text
Nice Values
```

like normal processes.

---

# Kubernetes

Kubernetes doesn't directly expose nice values.

Instead uses:

```text
CPU Requests

CPU Limits

QoS Classes
```

but underneath:

```text
Linux Scheduler
```

still manages CPU.

---

# Nice vs CPU Limits

Nice:

```text
Relative Preference
```

CPU Limit:

```text
Hard Restriction
```

Visual:

```text
Nice
  │
More CPU Preference
```

```text
CPU Limit
  │
Maximum CPU Allowed
```

---

# Troubleshooting High CPU

Step 1:

```bash
top
```

Check:

```text
CPU Usage
```

---

# Step 2

View nice values:

```bash
ps -eo pid,ni,cmd
```

---

# Step 3

Adjust if appropriate:

```bash
renice
```

---

# Common Mistakes

---

## ❌ Higher Nice = Higher Priority

Wrong.

Opposite.

---

## ❌ Nice Guarantees CPU

Wrong.

Only influences scheduler.

---

## ❌ Nice Controls Memory

Wrong.

CPU scheduling only.

---

## ❌ Nice Affects Real-Time Processes

Wrong.

Real-time scheduling ignores nice.

---

## ❌ Nice Fixes Bad Applications

Wrong.

Poorly written software remains poor.

---

# Useful Commands

---

## Show Nice Values

```bash
ps -eo pid,ni,cmd
```

---

## Start Process With Nice

```bash
nice -n 10 command
```

---

## Lowest Priority

```bash
nice -n 19 command
```

---

## Change Existing Process

```bash
renice 10 -p PID
```

---

## View Priority In top

```bash
top
```

Columns:

```text
PR

NI
```

---

# Interview Questions

## What is a nice value?

User-controlled scheduling priority adjustment.

---

## Nice Range?

```text
-20 to 19
```

---

## Highest Priority Nice Value?

```text
-20
```

---

## Default Nice Value?

```text
0
```

---

## Difference Between nice and renice?

nice:

```text
Start Process With Priority
```

renice:

```text
Change Existing Process Priority
```

---

## Does nice affect memory usage?

```text
No
```

---

## Does nice affect real-time scheduling?

```text
No
```

---

## Why are negative nice values restricted?

To prevent users from unfairly consuming CPU resources.

---

# Visual Summary

```text
Nice Values

-20  Highest Priority

  0  Default

 19  Lowest Priority
```

Flow:

```text
Nice Value
      │
      ▼
Scheduler Weight
      │
      ▼
CPU Share
```

Commands:

```bash
nice

renice

ps

top
```

Golden Rule:

```text
Lower Nice Number

      =

Higher Priority
```

---

# Final Takeaway

Linux scheduling is not simply:

```text
First Come

First Served
```

The scheduler constantly balances:

```text
Fairness

Performance

Responsiveness
```

Nice values provide a safe and powerful way to influence that balance.

```text
nice
```

is used when starting a process.

```text
renice
```

is used when adjusting a running process.

Understanding nice values is essential for running production systems where databases, web servers, background jobs, containers, and maintenance tasks must coexist without starving one another of CPU resources.
