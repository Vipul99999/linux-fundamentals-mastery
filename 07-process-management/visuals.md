# Linux Process Management Visual Atlas

> A visual-first guide to understanding Linux process management.
>
> If you can visualize it, you can understand it.
>
> This file contains architecture diagrams, process flows, lifecycle diagrams, scheduler visuals, container visuals, and troubleshooting maps.

---

# 1. Linux Process Management Big Picture

```text
                    Linux Kernel
                          │
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼

   Processes         Scheduling         Signals

        │                 │                 │

        └────────────┬────┴────┬────────────┘
                     │         │

                     ▼         ▼

                Containers   Threads
```

---

# 2. Program vs Process

```text
Program (Disk)

hello.c
hello.out

      │

      ▼

Execute

      │

      ▼

Process (Memory)

PID = 1000
State = Running
Memory = Allocated
CPU = Assigned
```

---

# 3. Process Lifecycle

```text
                fork()

                   │

                   ▼

             NEW PROCESS

                   │

                   ▼

                READY

                   │

          Scheduler Chooses

                   │

                   ▼

               RUNNING

             ┌─────┴─────┐
             │           │

             ▼           ▼

         WAITING      TERMINATED

             │
             ▼

          READY
```

---

# 4. Process State Diagram

```text
              +---------+
              |  READY  |
              +----+----+
                   |
                   |
                   ▼

              +---------+
              | RUNNING |
              +----+----+
                   |
       ┌───────────┼───────────┐
       │           │           │

       ▼           ▼           ▼

 +---------+  +---------+  +---------+
 |WAITING  |  |STOPPED  |  |EXITING  |
 +----+----+  +----+----+  +----+----+
      │            │            │
      ▼            ▼            ▼
   READY       RUNNING      ZOMBIE
```

---

# 5. Parent and Child Processes

```text
Parent Process
PID=100

      │

 fork()

      │

      ▼

Child Process
PID=200
PPID=100
```

---

# 6. fork() Visual

```text
Before

Process A

PID=100


After fork()

            PID=100
          Parent Process
                │
                │
                ▼
            PID=200
           Child Process
```

---

# 7. exec() Visual

```text
Before exec()

PID=200

bash


After exec()

PID=200

python app.py


PID remains same

Program changes
```

---

# 8. wait() Visual

```text
Parent
  │
  │ wait()
  ▼

Waiting

  ▲
  │

Child Exits

Exit Status Returned
```

---

# 9. Process Tree

```text
systemd (PID 1)

├── sshd
│     └── bash
│          └── vim
│
├── nginx
│     ├── worker
│     └── worker
│
└── docker
      └── container
```

---

# 10. PCB (Process Control Block)

```text
+----------------------------------+
| Process Control Block            |
+----------------------------------+
| PID                              |
| PPID                             |
| State                            |
| Program Counter                  |
| CPU Registers                    |
| Scheduling Info                  |
| Memory Info                      |
| Open Files                       |
| Signal Info                      |
+----------------------------------+
```

---

# 11. Context Switching

```text
CPU

Running Process A

      │

Timer Interrupt

      ▼

Save Registers

      ▼

Load Registers

      ▼

Running Process B
```

---

# 12. Context Switch Internals

```text
Process A

Registers
Stack Pointer
Program Counter

      │

      ▼

Saved Into PCB

      │

      ▼

PCB of Process B Loaded

      │

      ▼

Process B Continues
```

---

# 13. CPU Scheduler

```text
Ready Queue

+-----+
| P1  |
+-----+

+-----+
| P2  |
+-----+

+-----+
| P3  |
+-----+

+-----+
| P4  |
+-----+

      │

      ▼

Scheduler

      │

      ▼

CPU
```

---

# 14. CFS Scheduler Visualization

```text
Processes

P1
P2
P3
P4

      │

      ▼

Virtual Runtime Tree

          P2
         /  \
       P1    P4
             /
           P3

      │

Pick Smallest Runtime

      ▼

Run Next
```

---

# 15. Nice Values

```text
Priority Scale

High Priority

-20
-10
  0
 10
 19

Low Priority
```

---

# 16. CPU Share Visualization

```text
Nice 0

██████████████████


Nice 10

██████


Nice 19

██
```

---

# 17. Thread Model

```text
Process

Memory
Files
Resources

     │

     ├── Thread 1
     ├── Thread 2
     ├── Thread 3
     └── Thread 4
```

---

# 18. Process vs Threads

```text
PROCESS

+-------------------+
| Memory            |
| Files             |
+-------------------+


THREADS

+-------------------+
| Shared Memory     |
| Shared Files      |
+-------------------+

  T1  T2  T3  T4
```

---

# 19. Signal Architecture

```text
User

kill -TERM 1234

      │

      ▼

Kernel

      │

      ▼

Target Process

      │

      ▼

Signal Handler
```

---

# 20. Common Signals

```text
CTRL+C  ──► SIGINT

CTRL+Z  ──► SIGTSTP

kill     ──► SIGTERM

kill -9  ──► SIGKILL
```

---

# 21. Job Control

```text
Terminal

     │

     ▼

Shell

     │

 ┌───┼───┐

 ▼   ▼   ▼

Job1 Job2 Job3
```

---

# 22. Foreground vs Background

```text
Foreground

Keyboard
    │
    ▼
Process


Background

Process

(No Keyboard Access)
```

---

# 23. Zombie Process

```text
Parent Alive

      │

      ▼

Child Exits

      │

      ▼

Zombie

      │

wait()

      ▼

Removed
```

---

# 24. Zombie Visualization

```text
Memory      = Freed

CPU         = No

Files       = Closed

PID Entry   = Exists

State       = Z
```

---

# 25. Orphan Process

```text
Parent Dies

      │

      ▼

Child Still Running

      │

      ▼

Orphan

      │

      ▼

Adopted By PID 1
```

---

# 26. Zombie vs Orphan

```text
Zombie

Parent Alive
Child Dead


Orphan

Parent Dead
Child Alive
```

---

# 27. PID 1 Responsibilities

```text
systemd

   │

   ├── Adopt Orphans

   ├── Reap Zombies

   ├── Start Services

   └── Manage System
```

---

# 28. Namespace Overview

```text
Namespaces

├── PID
├── NET
├── MNT
├── IPC
├── UTS
├── USER
└── TIME
```

---

# 29. PID Namespace

```text
Host

PID 1
PID 100
PID 200


Container

PID 1
PID 2
PID 3
```

---

# 30. Network Namespace

```text
Container A

eth0
10.0.0.2


Container B

eth0
10.0.0.3
```

---

# 31. Mount Namespace

```text
Host

/
├── home
├── var
└── etc


Container

/
├── app
├── bin
└── etc
```

---

# 32. Cgroups

```text
Process

     │

     ▼

Cgroup

     │

 ┌───┼────┐

 ▼   ▼    ▼

CPU RAM  IO
```

---

# 33. CPU Limit

```text
Server

64 CPUs


Container

Limit = 2 CPUs
```

---

# 34. Memory Limit

```text
Container

Limit = 512MB


██████████

Limit Reached

OOM Kill
```

---

# 35. Docker Architecture

```text
Docker CLI

      │

Docker Engine

      │

containerd

      │

runc

      │

Linux Kernel
```

---

# 36. Container Internals

```text
Container

├── Namespaces
├── Cgroups
├── Root Filesystem
└── Process
```

---

# 37. Container PID 1 Problem

```text
Container

PID 1

Application

     │

Children

     │

Zombie?

     ▼

Must Reap
```

---

# 38. Kubernetes Architecture

```text
Kubernetes

      │

Container Runtime

      │

Linux Kernel

      │

Namespaces
+
Cgroups
```

---

# 39. Kubernetes Pod

```text
Pod

├── Pause Container
├── App Container
└── Sidecar Container
```

---

# 40. Pause Container

```text
Pause Container

      │

Holds

Network Namespace
IPC Namespace
```

---

# 41. Process Troubleshooting Flow

```text
System Slow

     │

     ▼

top / htop

     │

     ├── CPU High?
     ├── RAM High?
     ├── Zombie?
     └── Load High?

     ▼

Root Cause
```

---

# 42. Process Investigation Map

```text
Problem

     │

     ▼

ps

     │

     ▼

PID

     │

     ▼

Parent

     │

     ▼

Signals

     │

     ▼

Fix
```

---

# 43. Complete Linux Process Universe

```text
                         Linux Kernel
                                │
                                │
 ┌──────────────────────────────┼──────────────────────────────┐
 │                              │                              │

 ▼                              ▼                              ▼

Processes                  Scheduling                     Signals

 │                              │                              │

 ▼                              ▼                              ▼

Threads                   Priorities                     Handlers

 │                              │                              │

 ▼                              ▼                              ▼

fork()                    Nice Values                    SIGTERM

exec()                    CFS                           SIGKILL

wait()                    CPU Time                      SIGINT

 │
 ▼

Zombie
Orphan

 │
 ▼

Namespaces

 │
 ▼

Containers

 │
 ▼

Docker

 │
 ▼

Kubernetes
```

---

# Final Mental Model

```text
Program
   │
   ▼
Process
   │
   ├── Threads
   ├── Signals
   ├── Scheduling
   ├── Memory
   ├── Files
   └── Networking
         │
         ▼
Namespaces + Cgroups
         │
         ▼
Containers
         │
         ▼
Docker / Kubernetes
```

---

# One-Screen Summary

```text
Process
   │
   ▼
fork()
   │
   ▼
Child
   │
   ▼
exec()
   │
   ▼
Running
   │
   ├── Signals
   ├── Threads
   ├── Scheduling
   ├── Nice
   └── Resources
   │
   ▼
exit()
   │
   ▼
Zombie
   │
wait()
   ▼
Removed

Parent Dies
   │
   ▼
Orphan
   │
PID 1
   ▼
Adopted

Namespaces
+
Cgroups
   ▼
Containers
   ▼
Docker
   ▼
Kubernetes
```
