# CIDR (Classless Inter-Domain Routing)

> Learn how modern networks are designed, how Linux understands network boundaries, and why CIDR powers cloud computing, Docker, Kubernetes, and today's internet.

---

# Why Learn CIDR?

One of the biggest questions in networking is:

> How do we divide billions of devices efficiently?

Old networking had problems.

Networks were either:

```text
Too Big

or

Too Small
```

CIDR solved this.

Today almost every infrastructure depends on CIDR.

Examples:

```text
Linux

AWS

Azure

GCP

Docker

Kubernetes

Datacenters

Enterprise Networks
```

---

# What Problem Did CIDR Solve?

Before CIDR, networking used classes.

```text
Class A

Class B

Class C
```

Problem:

Huge waste.

Example:

Company needs:

```text
500 devices
```

Class C:

```text
256 devices

❌ Too small
```

Class B:

```text
65,536 devices

❌ Too large
```

Huge waste.

CIDR solved this.

---

# Simple Definition

CIDR is:

> A method of allocating IP addresses efficiently without relying on rigid network classes.

---

# Mental Model

CIDR answers:

```text
How much of the address is network?

↓

How much is host?
```

---

# Big Picture

```text
IP Address

+

Prefix Length

=

CIDR
```

Example:

```text
192.168.1.0/24
```

---

# What Does /24 Mean?

It means:

```text
24 bits

↓

Network Portion
```

Remaining:

```text
8 bits

↓

Host Portion
```

---

# Visualization

```text
192.168.1.25/24

↓

192.168.1 | 25

↓

Network | Host
```

---

# Binary Visualization

```text
192.168.1.25

↓

11000000

10101000

00000001

00011001
```

/24 means:

```text
11111111

11111111

11111111

00000000
```

---

# Prefix Length Is The Core Idea

CIDR is all about prefixes.

More prefix bits:

```text
Smaller network
```

Fewer prefix bits:

```text
Larger network
```

---

# Visualization

```text
/8

Huge


/16

Large


/24

Medium


/28

Small


/30

Tiny
```

---

# Think Like A Pizza

Suppose:

```text
Whole Pizza
```

Cut into pieces.

Large slices:

```text
Fewer networks

More hosts
```

Small slices:

```text
More networks

Fewer hosts
```

CIDR works similarly.

---

# CIDR Table

| Prefix | Total Addresses |
|--------|----------------|
| /8 | 16,777,216 |
| /16 | 65,536 |
| /24 | 256 |
| /25 | 128 |
| /26 | 64 |
| /27 | 32 |
| /28 | 16 |
| /29 | 8 |
| /30 | 4 |
| /32 | 1 |

---

# Why Does This Matter?

Because engineers design networks around these sizes.

Examples:

```text
Home Network

↓

/24
```

---

```text
Cloud Subnet

↓

/24
```

---

```text
Point-To-Point Links

↓

/30
```

---

# Visual Example

Suppose:

```text
192.168.1.0/24
```

Visualization:

```text
Network

192.168.1.0

↓

Hosts

192.168.1.1

↓

192.168.1.254

↓

Broadcast

192.168.1.255
```

---

# CIDR Replaced Classes

Old:

```text
Class A

Class B

Class C
```

Modern:

```text
/8

/16

/24

/26

/28

Choose whatever you need
```

Much better.

---

# Why Engineers Love CIDR

Advantages:

```text
Flexible

Efficient

Scalable

Less waste

Easy to organize
```

---

# Route Aggregation (Super Important)

CIDR also helps routers.

Without aggregation:

```text
1000 routes
```

Router struggles.

---

With aggregation:

```text
1 summarized route
```

Much better.

---

# Visualization

Without CIDR:

```text
10.0.1.0

10.0.2.0

10.0.3.0

10.0.4.0
```

---

Summarized:

```text
10.0.0.0/16
```

Smaller routing tables.

---

# Linux Perspective ⭐⭐⭐

Linux uses CIDR everywhere.

Examples:

```bash
ip addr
```

Output:

```text
192.168.1.10/24
```

Linux immediately understands:

```text
IP

+

Network Boundary
```

---

# Linux Internals ⭐⭐⭐

Suppose:

```text
curl github.com
```

Linux internally performs:

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

CIDR Check ⭐

↓

Same Network?

↓

Different Network?

↓

Gateway?

↓

Interface?

↓

Transmit
```

---

# Linux Route Decision Example

Suppose:

```text
192.168.1.10/24
```

Destination:

```text
192.168.1.20
```

Linux asks:

```text
Same subnet?
```

YES

↓

Direct communication.

---

Destination:

```text
8.8.8.8
```

Linux asks:

```text
Different subnet?
```

YES

↓

Gateway.

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

# Linux Longest Prefix Match ⭐⭐⭐⭐⭐

This is extremely important.

Suppose Linux has:

```text
10.0.0.0/8

10.1.0.0/16

10.1.1.0/24
```

Destination:

```text
10.1.1.50
```

Linux chooses:

```text
10.1.1.0/24
```

because:

```text
Most specific route wins.
```

This is called:

```text
Longest Prefix Match
```

---

# Visualization

```text
General

10.0.0.0/8

↓

More Specific

10.1.0.0/16

↓

Most Specific

10.1.1.0/24
```

Linux chooses:

```text
/24
```

---

# Modern Infrastructure Usage

---

# AWS

Example:

```text
10.0.0.0/16

↓

10.0.1.0/24

↓

10.0.2.0/24
```

---

# Azure

Uses CIDR heavily.

---

# GCP

Uses CIDR heavily.

---

# Docker

Default:

```text
172.17.0.0/16
```

---

# Kubernetes

Examples:

```text
10.244.0.0/16

10.96.0.0/12
```

---

# Real Production Example

Company infrastructure:

```text
Internet

↓

Load Balancer

↓

10.0.1.0/24

↓

Application

↓

10.0.2.0/24

↓

Database
```

CIDR everywhere.

---

# Security Perspective

CIDR also improves security.

Good:

```text
Internet

↓

Public Subnet

↓

Private Subnet

↓

Database Subnet
```

Bad:

```text
Internet

↓

Database
```

---

# Troubleshooting

Problem:

```text
Cannot access printer.
```

Ask:

```text
Same subnet?
```

---

Problem:

```text
Cannot access internet.
```

Ask:

```text
Gateway configured?
```

---

Problem:

```text
Cloud resources cannot communicate.
```

Ask:

```text
CIDR overlap?
```

Very common.

---

# CIDR Overlap Problem ⭐⭐⭐⭐⭐

Bad:

```text
VPC A

10.0.0.0/16

VPC B

10.0.0.0/16
```

VPN breaks.

Peering breaks.

Routing breaks.

Avoid overlaps.

---

# Linux Commands

View IPs:

```bash
ip addr
```

---

View routes:

```bash
ip route
```

---

Show interfaces:

```bash
ip link
```

---

# Common Misconceptions

❌ CIDR = Subnetting

Wrong.

CIDR is more flexible.

---

❌ Bigger prefix means bigger network

Wrong.

```text
Bigger prefix

↓

Smaller network
```

---

❌ CIDR only matters in cloud

Wrong.

Linux uses it everywhere.

---

# Engineer Mental Model

Always think:

```text
IP

↓

Boundary

↓

Same Network?

↓

Gateway?

↓

Route?

↓

Destination
```

---

# Visual Summary

```text
Large Network

↓

CIDR

↓

Flexible Networks

↓

Efficient Routing

↓

Scalable Infrastructure
```

---

# WH Questions

## What is CIDR?

Flexible IP allocation.

---

## Why was CIDR invented?

To replace inefficient classes.

---

## Why do Linux engineers use it?

Linux networking depends on prefixes.

---

## Why do cloud providers use it?

Infrastructure organization.

---

## Why does Docker use it?

Container isolation.

---

## Why does Kubernetes use it?

Pod networking.

---

# Key Takeaways

Remember forever.

```text
IP

+

Prefix

↓

CIDR

↓

Flexible Networks

↓

Scalable Infrastructure
```

---

# What's Next?

```text
ipv6.md
```

This file will answer one of the biggest modern networking questions:

> Why wasn't IPv4 enough?
