# Subnetting

> Learn how large networks are divided into smaller networks to improve performance, scalability, security, and manageability.

---

# Why Learn Subnetting?

Imagine a company with:

```text
10,000 devices
```

all inside one network.

Problems appear immediately.

```text
Broadcast traffic

↓

Congestion

↓

Security issues

↓

Difficult management

↓

Poor scalability
```

Subnetting solves this.

---

# What Problem Does Subnetting Solve?

Without subnetting:

```text
Everyone

↓

Talks to Everyone
```

Chaos.

Subnetting creates organization.

---

# Simple Definition

Subnetting is:

> The process of dividing one large network into multiple smaller networks.

---

# Simple Analogy

Imagine one giant office.

Without rooms:

```text
10,000 employees

↓

One giant hall
```

Problems:

```text
Noise

Congestion

No organization
```

Instead:

```text
Engineering Room

HR Room

Finance Room

Security Room
```

Networking works the same way.

---

# Big Picture

Without subnetting:

```text
Internet

↓

Company

↓

10,000 Devices
```

With subnetting:

```text
Company

↓

Engineering

↓

HR

↓

Finance

↓

Security
```

---

# Why Companies Use Subnetting

Examples:

```text
Engineering Team

10.0.1.0

↓

HR Team

10.0.2.0

↓

Finance Team

10.0.3.0

↓

Security Team

10.0.4.0
```

---

# Core Idea

Take one network.

Split it.

Visualization:

```text
192.168.1.0

↓

192.168.1.0

192.168.1.64

192.168.1.128

192.168.1.192
```

---

# Why Split Networks?

Five major reasons.

---

# 1. Reduce Broadcast Traffic

Without subnetting:

```text
5000 devices

↓

Broadcast

↓

5000 devices receive it
```

Wasteful.

---

# With Subnetting

```text
Engineering

500 devices

↓

Broadcast

↓

500 devices receive it
```

Much better.

---

# 2. Improve Security

Without subnetting:

```text
Everyone

↓

Everyone
```

Dangerous.

---

With subnetting:

```text
Finance

↓

Firewall

↓

Engineering
```

Controlled access.

---

# 3. Improve Performance

Smaller networks:

```text
Less noise

Less congestion

Faster communication
```

---

# 4. Better Organization

Instead of:

```text
10,000 devices
```

Use:

```text
Departments

Teams

Applications

Services
```

---

# 5. Better Troubleshooting

Instead of:

```text
Entire company broken
```

You can isolate:

```text
Finance subnet broken
```

---

# What Is A Subnet?

Subnet means:

```text
Sub Network
```

Simply:

```text
Smaller network
```

---

# Visual Example

Original:

```text
192.168.1.0
```

Split into:

```text
192.168.1.0

192.168.1.64

192.168.1.128

192.168.1.192
```

---

# Every Subnet Has These Components

```text
Network Address

↓

Usable Hosts

↓

Broadcast Address
```

---

# Visualization

```text
Network

↓

Devices

↓

Broadcast
```

---

# Network Portion vs Host Portion

This concept is critical.

Example:

```text
192.168.1.10/24
```

Visualization:

```text
192.168.1 | 10

Network | Host
```

---

# Why Is This Important?

Network:

```text
Where are we?
```

Host:

```text
Which device?
```

---

# Subnet Mask

Subnet masks tell us:

```text
Which bits are network bits?

Which bits are host bits?
```

---

# Example

```text
255.255.255.0
```

Visualization:

```text
11111111

11111111

11111111

00000000
```

---

# Meaning

```text
24 network bits

8 host bits
```

---

# CIDR Notation

Instead of:

```text
255.255.255.0
```

we write:

```text
/24
```

Much easier.

---

# Visualization

```text
192.168.1.10/24
```

means:

```text
24 network bits

8 host bits
```

---

# Bigger Prefix = Smaller Network

This is extremely important.

---

# Visualization

```text
/8

Huge Network


/16

Smaller


/24

Smaller


/28

Very Small
```

---

# Example

```text
10.0.0.0/8

16 million addresses
```

---

```text
192.168.1.0/24

256 addresses
```

---

```text
192.168.1.0/28

16 addresses
```

---

# Why This Happens

More network bits:

```text
Less host space
```

---

# Broadcast Domains

Without subnetting:

```text
1000 devices

↓

One broadcast domain
```

---

With subnetting:

```text
250

250

250

250
```

Much better.

---

# Linux Perspective ⭐⭐⭐

Linux itself doesn't "do subnetting."

Linux understands network prefixes.

Linux uses them to make routing decisions.

---

# View Address

```bash
ip addr
```

Example:

```text
192.168.1.10/24
```

---

# Linux Understands

```text
IP

+

Prefix Length
```

---

# Linux Internal Visualization ⭐⭐⭐

Suppose:

```text
curl github.com
```

Linux internally:

```text
Application

↓

TCP

↓

IP

↓

Routing Table

↓

Subnet Check ⭐

↓

Gateway?

↓

Interface?

↓

Transmit
```

---

# Linux Route Decision

Suppose:

```text
192.168.1.10
```

wants:

```text
192.168.1.20
```

Linux checks:

```text
Same subnet?
```

YES

↓

Send directly

---

Suppose:

```text
192.168.1.10
```

wants:

```text
8.8.8.8
```

Linux checks:

```text
Different subnet?
```

YES

↓

Use gateway

---

# Visualization

Same subnet:

```text
Laptop

↓

Switch

↓

Printer
```

Different subnet:

```text
Laptop

↓

Router

↓

Internet
```

---

# Modern Infrastructure Usage

---

# Office Networks

```text
HR

↓

Engineering

↓

Finance

↓

Security
```

---

# Cloud

AWS VPC:

```text
10.0.0.0/16

↓

10.0.1.0/24

↓

10.0.2.0/24
```

---

# Docker

```text
172.17.0.0/16
```

---

# Kubernetes

```text
10.x.x.x
```

subnets everywhere.

---

# Real Production Example

Large company.

```text
Employees

5000
```

Without subnetting:

```text
One network
```

Disaster.

Instead:

```text
Users

↓

Apps

↓

Databases

↓

Security Zones
```

Separate subnets.

---

# Security Perspective

Subnetting is also security.

Example:

Good:

```text
Internet

↓

Load Balancer

↓

Application Subnet

↓

Database Subnet
```

Bad:

```text
Internet

↓

Database
```

Never expose databases unnecessarily.

---

# Troubleshooting

Suppose:

```text
Cannot reach printer.
```

Ask:

```text
Same subnet?
```

---

Suppose:

```text
Cannot access internet.
```

Ask:

```text
Gateway configured?
```

---

# Linux Commands

View interfaces:

```bash
ip addr
```

---

View routes:

```bash
ip route
```

---

Ping:

```bash
ping
```

---

# Common Misconceptions

❌ Subnetting is only math.

Wrong.

Subnetting is network organization.

---

❌ Bigger prefix means bigger network.

Wrong.

```text
Bigger prefix

↓

Smaller network
```

---

# Engineer Mental Model

Always think:

```text
Large Network

↓

Split

↓

Organize

↓

Secure

↓

Scale
```

---

# Visual Summary

```text
One Giant Network

↓

Subnetting

↓

Smaller Networks

↓

Performance

↓

Security

↓

Scalability
```

---

# WH Questions

## What is subnetting?

Dividing a large network into smaller ones.

---

## Why does subnetting exist?

To organize networks.

---

## Why reduce broadcasts?

Performance improves.

---

## Why do cloud providers use subnetting?

Isolation and scalability.

---

## Why does Linux care?

Routing decisions depend on subnets.

---

## Why do engineers care?

Everything modern infrastructure builds on subnets.

---

# Key Takeaways

Remember forever.

```text
Large Network

↓

Smaller Networks

↓

Less Broadcast

↓

More Security

↓

Better Performance
```

---

# What's Next?

```text
cidr.md
```

This file is extremely important because CIDR powers modern networking.
