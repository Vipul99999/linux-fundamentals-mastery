# Linux Dependency Management Deep Fundamentals

> Understanding how Linux transforms thousands of independent components into a single functioning operating system.

---

# Learning Goals

By the end of this file, you will understand:

- Why dependency management exists
- Why Linux is a dependency graph
- How systemd orchestrates dependencies
- Hard vs soft dependencies
- Ordering vs requirement dependencies
- Boot dependency management
- Failure propagation
- Dependency trees
- Circular dependency problems
- Parallel execution
- Production dependency design
- Cloud infrastructure dependency management

---

# First Principles

Imagine Linux boots.

Question:

Can applications start randomly?

Imagine:

```text
Nginx

↓

API

↓

PostgreSQL

↓

Network
```

If nginx starts first:

```text
Nginx

↓

Cannot reach API

↓

Website fails
```

Dependencies exist everywhere.

---

# The Biggest Idea

Linux is not:

```text
1000 independent programs
```

Linux is:

> A dependency graph.

Everything depends on something else.

---

# Human Analogy

Imagine a restaurant.

You cannot do this:

```text
Serve food

↓

Cook food

↓

Buy ingredients
```

Correct:

```text
Buy ingredients

↓

Cook food

↓

Serve food
```

Linux works exactly the same way.

---

# Mental Model

```text
Linux = City

systemd = Mayor

Services = Buildings

Dependencies = Roads
```

Without roads:

```text
Chaos
```

---

# The Dependency Problem

Linux contains:

```text
Kernel

Networking

Docker

Redis

PostgreSQL

Nginx

Applications

Monitoring
```

Question:

Who coordinates everything?

Answer:

```text
systemd
```

---

# Linux Is A Graph

Not a list.

Wrong:

```text
Nginx

Docker

Redis

PostgreSQL
```

Correct:

```mermaid
flowchart TD

Network

Network --> Docker

Network --> PostgreSQL

Docker --> API

PostgreSQL --> API

API --> Nginx

Nginx --> Monitoring
```

---

# The Dependency Solver

systemd acts like a graph solver.

Its job:

```text
Read unit files

↓

Build dependency graph

↓

Detect relationships

↓

Resolve order

↓

Execute services

↓

Monitor failures
```

---

# Architecture

```mermaid
flowchart TD

UnitFiles

UnitFiles --> DependencyGraph

DependencyGraph --> GraphSolver

GraphSolver --> ExecutionPlan

ExecutionPlan --> LinuxSystem
```

---

# Dependency Categories

There are two major categories.

```text
Ordering Dependencies

Requirement Dependencies
```

---

# Ordering Dependencies

Question:

> Who goes first?

Examples:

```text
After=

Before=
```

---

# Requirement Dependencies

Question:

> Who must exist?

Examples:

```text
Requires=

Wants=

BindsTo=

Requisite=
```

---

# Visualization

```mermaid
flowchart TD

Dependencies

Dependencies --> Ordering

Dependencies --> Requirement

Ordering --> After

Ordering --> Before

Requirement --> Requires

Requirement --> Wants

Requirement --> BindsTo
```

---

# Layered Linux Architecture

Linux boots in layers.

```mermaid
flowchart TD

Kernel

Kernel --> systemd

systemd --> Filesystems

Filesystems --> Networking

Networking --> Databases

Databases --> Applications

Applications --> Monitoring
```

---

# Dependency Types

There are four major concepts.

```text
Ordering

Requirement

Propagation

Exclusion
```

---

# Ordering

Question:

Who starts first?

Example:

```ini
After=network.target
```

---

# Requirement

Question:

Who is mandatory?

Example:

```ini
Requires=postgresql.service
```

---

# Propagation

Question:

What else should stop?

Example:

```ini
PartOf=application-stack.target
```

---

# Exclusion

Question:

Who cannot coexist?

Example:

```ini
Conflicts=rescue.target
```

---

# Hard Dependencies

Hard means:

```text
Must exist
```

Visual:

```mermaid
flowchart TD

Database

Database --> API

DatabaseX[Database Failure]

DatabaseX --> APIFailure
```

---

# Soft Dependencies

Soft means:

```text
Useful but optional
```

Visual:

```mermaid
flowchart TD

API

API --> Monitoring

MonitoringX[Monitoring Failure]

API --> StillHealthy
```

---

# Dependency Directives

Important directives.

| Directive | Purpose |
|-----------|---------|
| After | Ordering |
| Before | Reverse ordering |
| Requires | Mandatory |
| Wants | Optional |
| BindsTo | Lifecycle coupling |
| PartOf | Propagation |
| Requisite | Must already exist |
| Conflicts | Mutual exclusion |

---

# The Golden Pattern

Very common.

```ini
[Unit]

Requires=postgresql.service

After=postgresql.service
```

Why both?

Because they solve different problems.

```text
Requires

↓

Existence

---------------

After

↓

Order
```

---

# Visual

```mermaid
flowchart TD

Existence

Order

Existence --> HealthyStartup

Order --> HealthyStartup
```

---

# Boot Dependency Management

Boot is a giant dependency graph.

Visual:

```mermaid
flowchart TD

Firmware

Firmware --> Bootloader

Bootloader --> Kernel

Kernel --> systemd

systemd --> basic.target

basic.target --> multi-user.target

multi-user.target --> Services
```

---

# Production Web Stack Example

Components:

```text
Network

PostgreSQL

Redis

API

Nginx
```

Visual:

```mermaid
flowchart TD

Network

Network --> PostgreSQL

Network --> Redis

PostgreSQL --> API

Redis --> API

API --> Nginx
```

---

# Dependency Trees

Every service belongs to a tree.

Example:

```mermaid
flowchart TD

network.target

network.target --> docker.service

docker.service --> application.service

application.service --> monitoring.service
```

---

# Reverse Dependencies

Question:

Who needs nginx?

Visual:

```mermaid
flowchart TD

Frontend

Monitoring

Frontend --> Nginx

Monitoring --> Nginx
```

Commands:

```bash
systemctl list-dependencies nginx
```

Reverse:

```bash
systemctl list-dependencies --reverse nginx
```

---

# Failure Propagation

Question:

What happens when databases fail?

Visual:

```mermaid
flowchart TD

Database

Database --> API

API --> Nginx

DatabaseX[Database Failure]

DatabaseX --> APIFailure

APIFailure --> NginxFailure
```

This is cascading failure.

---

# Cascading Failures

One broken dependency can destroy systems.

Visual:

```mermaid
flowchart TD

DNS

DNS --> API

API --> Nginx

API --> Monitoring

DNSX[DNS Failure]

DNSX --> EntireSystemFailure
```

---

# Parallel Execution

Old systems:

```text
A

↓

B

↓

C
```

Modern systemd:

```mermaid
flowchart TD

Network

Network --> PostgreSQL

Network --> Redis

Network --> Docker
```

Independent services start simultaneously.

---

# Why Parallelization Matters

Benefits:

```text
Faster boot

Better scalability

Higher efficiency
```

---

# Circular Dependencies

Dangerous.

Wrong:

```mermaid
flowchart TD

A --> B

B --> C

C --> A
```

Impossible to solve.

---

# Detect Cycles

Useful command:

```bash
systemd-analyze verify myapp.service
```

---

# Visualize Entire System

```bash
systemd-analyze dot
```

Export:

```bash
systemd-analyze dot > graph.dot
```

Convert:

```bash
dot -Tpng graph.dot -o graph.png
```

---

# Cloud Infrastructure Example

AWS VM.

Visual:

```mermaid
flowchart TD

systemd

systemd --> cloud-init

cloud-init --> Networking

Networking --> Docker

Docker --> Applications
```

---

# Kubernetes Example

Kubernetes itself relies on dependency management.

```mermaid
flowchart TD

Network

Network --> containerd

containerd --> kubelet

kubelet --> Pods
```

---

# Production Stack Example

```mermaid
flowchart TD

Network

Network --> PostgreSQL

Network --> Redis

PostgreSQL --> API

Redis --> API

API --> Nginx

Nginx --> Monitoring
```

---

# Troubleshooting Workflow

Question:

Application won't start.

Step 1

Inspect.

```bash
systemctl status app
```

Step 2

Dependencies.

```bash
systemctl list-dependencies app
```

Step 3

Reverse dependencies.

```bash
systemctl list-dependencies --reverse app
```

Step 4

Logs.

```bash
journalctl -u app
```

Step 5

Find failures.

```bash
systemctl --failed
```

---

# Dependency Design Principles

Good systems should be:

```text
Simple

Loose coupled

Observable

Recoverable

Predictable
```

---

# Common Beginner Mistakes

## Mistake 1

Thinking Linux is a list.

Wrong.

Linux is a graph.

---

## Mistake 2

Using only:

```ini
After=
```

Wrong.

It does not create requirements.

---

## Mistake 3

Building circular dependencies.

Very dangerous.

---

## Mistake 4

Making everything mandatory.

Creates fragile systems.

---

# Engineering Mindset

Do not think:

```text
Linux starts services
```

Think:

```text
Linux solves dependency graphs
```

That is much closer to reality.

---

# Mental Model To Remember Forever

```text
Unit Files

↓

Dependency Graph

↓

systemd

↓

Execution Plan

↓

Linux
```

Or even simpler:

```text
Operating systems are dependency graphs.

systemd is the graph solver.
```

That single sentence explains modern Linux orchestration.
