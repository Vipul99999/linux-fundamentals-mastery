
# References

> Great engineers don't memorize technologies. They continuously build mental models.

This file is a curated knowledge map for becoming a world-class Cloud, Linux, Infrastructure, and Systems Engineer.

---

# Learning Philosophy

Do NOT learn in this order.

❌ Wrong:

```text
AWS

↓

Azure

↓

GCP
```

Learn in this order.

✅ Correct:

```text
Linux

↓

Networking

↓

Storage

↓

Security

↓

Virtualization

↓

Cloud

↓

Containers

↓

Kubernetes

↓

Distributed Systems

↓

Platform Engineering

↓

System Design
```

Cloud is built on top of fundamentals.

---

# Tier 1: Linux Fundamentals (Mandatory)

Learn these deeply first.

## Topics

```text
Linux Architecture

Processes

Memory

Storage

Filesystems

Networking

Permissions

Systemd

Bash

Observability
```

---

## Books

### Linux Bible

Author:

```text
Christopher Negus
```

---

### How Linux Works

Author:

```text
Brian Ward
```

---

### The Linux Programming Interface

Author:

```text
Michael Kerrisk
```

Advanced but incredible.

---

### UNIX and Linux System Administration Handbook

Authors:

```text
Nemeth

Snyder

Hein

Whaley
```

---

# Tier 2: Computer Networking (Mandatory)

## Topics

```text
TCP/IP

Routing

DNS

CIDR

Subnets

HTTP

TLS

Load Balancing
```

---

## Books

### Computer Networking: A Top Down Approach

Authors:

```text
Kurose

Ross
```

---

### TCP/IP Illustrated Volume 1

Author:

```text
W. Richard Stevens
```

Advanced.

---

# Tier 3: Cloud Foundations

Learn concepts.

Not services.

## Topics

```text
IaaS

PaaS

SaaS

VPC

Subnets

IAM

Load Balancing

Autoscaling

Cloud Architecture
```

---

# Official Cloud Documentation

## AWS

Documentation:

https://docs.aws.amazon.com

Architecture Center:

https://aws.amazon.com/architecture

Well Architected Framework:

https://aws.amazon.com/architecture/well-architected

---

## Azure

Documentation:

https://learn.microsoft.com/azure

Architecture Center:

https://learn.microsoft.com/azure/architecture

Well Architected Framework:

https://learn.microsoft.com/azure/well-architected

---

## GCP

Documentation:

https://cloud.google.com/docs

Architecture Framework:

https://cloud.google.com/architecture/framework

---

# Tier 4: Storage Systems

## Topics

```text
Block Storage

File Storage

Object Storage

Replication

Durability

Data Lakes
```

---

## Books

### Designing Data Intensive Applications

Author:

```text
Martin Kleppmann
```

Mandatory book.

---

## Topics To Focus On

```text
Replication

Partitioning

Transactions

Distributed Systems
```

---

# Tier 5: Security Engineering

## Topics

```text
IAM

RBAC

ABAC

Zero Trust

Authentication

Authorization

Secrets Management
```

---

## Books

### Security Engineering

Author:

```text
Ross Anderson
```

---

## Zero Trust

### BeyondCorp Papers

Read Google's papers.

---

# Tier 6: Virtualization

## Topics

```text
Hypervisors

Virtual Machines

Containers

Namespaces

cgroups
```

---

## Resources

### VMware Learning Resources

### KVM Documentation

### QEMU Documentation

---

# Tier 7: Containers

## Topics

```text
Docker

containerd

runc

OCI

OverlayFS
```

---

## Official Resources

Docker:

https://docs.docker.com

containerd:

https://containerd.io/docs

OCI:

https://opencontainers.org

---

# Tier 8: Kubernetes

## Topics

```text
Pods

Nodes

Services

Ingress

Networking

Storage

Autoscaling
```

---

## Official Documentation

Kubernetes:

https://kubernetes.io/docs

---

## Books

### Kubernetes Up and Running

Authors:

```text
Kelsey Hightower

Brendan Burns

Joe Beda
```

---

# Tier 9: Distributed Systems

## Topics

```text
Consensus

Replication

Leader Election

CAP Theorem

Fault Tolerance

Consistency
```

---

## Books

### Designing Distributed Systems

Author:

```text
Brendan Burns
```

---

### Distributed Systems

Author:

```text
Maarten Van Steen
```

---

# Tier 10: Site Reliability Engineering

## Topics

```text
Reliability

SLI

SLO

SLA

Error Budgets

Observability
```

---

## Books

### Site Reliability Engineering

Google

Free online.

---

### The Site Reliability Workbook

Google

Free online.

---

# Tier 11: Platform Engineering

## Topics

```text
Internal Developer Platforms

Self Service Infrastructure

GitOps

Developer Experience
```

---

## Books

### Platform Engineering

Authors:

```text
Camille Fournier

Ian Nowland

Richard Seroter
```

---

# Tier 12: System Design

## Topics

```text
Scalability

Availability

Latency

Caching

Queues

Microservices

Global Systems
```

---

## Books

### System Design Interview

Author:

```text
Alex Xu
```

Good for beginners.

---

### Fundamentals Of Software Architecture

Authors:

```text
Mark Richards

Neal Ford
```

Excellent.

---

# Tier 13: AI Infrastructure (Future)

## Topics

```text
GPU Clusters

AI Pipelines

Vector Databases

Inference Systems

AI Agents
```

---

## Learn

```text
PyTorch

Ray

vLLM

KServe
```

---

# Linux Commands To Master

Storage:

```bash
lsblk

df

du

mount

fdisk
```

Networking:

```bash
ip

ss

ping

traceroute

tcpdump
```

Processes:

```bash
ps

top

htop

kill
```

Observability:

```bash
journalctl

vmstat

iostat

sar
```

---

# Linux Concepts To Master Before Kubernetes

```text
Processes

Threads

Memory

Storage

Filesystems

Networking

Security

Systemd

Containers
```

If Linux is weak, Kubernetes will feel impossible.

---

# Cloud Engineer Roadmap

```text
Linux

↓

Networking

↓

Storage

↓

Security

↓

Virtualization

↓

Cloud

↓

Containers

↓

Kubernetes

↓

Distributed Systems

↓

SRE

↓

Platform Engineering

↓

System Architect
```

---

# Founder Roadmap

```text
Infrastructure

↓

Architecture

↓

Reliability

↓

Efficiency

↓

Cost Optimization

↓

Business Sustainability
```

---

# YouTube Channels

## Linux

```text
Learn Linux TV

The Linux Foundation

NetworkChuck
```

---

## System Design

```text
ByteByteGo

Gaurav Sen

Jordan has no life
```

---

## Infrastructure

```text
TechWorld with Nana

KodeKloud

Bret Fisher
```

---

# GitHub Repositories To Explore

```text
awesome-linux

awesome-cloud-native

awesome-kubernetes

awesome-scalability
```

---

# Research Topics (Advanced)

Study these eventually.

```text
Google Borg

Google Spanner

MapReduce

BigTable

Kafka

Raft

Paxos
```

---

# Engineering Growth Pyramid

```text
Linux

↓

Networking

↓

Storage

↓

Security

↓

Cloud

↓

Containers

↓

Kubernetes

↓

Distributed Systems

↓

SRE

↓

Platform Engineering

↓

System Architecture

↓

Founder Thinking
```

---

# Final Advice

Do not become:

```text
AWS Engineer
```

Become:

```text
Linux Engineer

↓

Infrastructure Engineer

↓

Cloud Engineer

↓

Distributed Systems Engineer

↓

System Architect
```

Cloud providers will change.

Fundamentals will not.

---

# Final Thought

Everything eventually comes back to:

```text
CPU

↓

Memory

↓

Storage

↓

Network

↓

Linux

↓

Distributed Systems
```

Master these deeply.

The entire modern internet is built on top of them.
