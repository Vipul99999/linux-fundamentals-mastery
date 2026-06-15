# ARP vs NDP (Neighbor Discovery Protocol)

> Learn why IPv6 replaced ARP, how Linux evolved from ARP to NDP, and how modern infrastructures discover neighbors.

---

# Why This File Exists

One of the biggest networking questions is:

```text
IPv4

↓

ARP


IPv6

↓

???
```

Many beginners assume:

```text
IPv6

↓

ARP
```

Wrong.

IPv6 does NOT use ARP.

It uses:

```text
NDP

(Neighbor Discovery Protocol)
```

---

# The Big Question

Suppose:

```text
IPv6 Device

↓

Needs MAC Address
```

Question:

```text
How does it find it?
```

Answer:

```text
NDP
```

---

# Simple Definitions

## ARP

```text
IPv4

↓

IP → MAC
```

---

## NDP

```text
IPv6

↓

Neighbor Discovery System
```

NDP does much more than ARP.

---

# Mental Model ⭐⭐⭐⭐⭐

Think:

```text
ARP

↓

One Tool


NDP

↓

Toolbox
```

---

# Why Was ARP Replaced?

ARP had limitations.

Problems:

```text
Broadcast Traffic

Limited Features

No Auto Configuration

No Router Discovery

No Duplicate Detection

Weak Design
```

IPv6 redesigned everything.

---

# Big Picture

IPv4:

```text
IP

↓

ARP

↓

MAC
```

IPv6:

```text
IPv6

↓

NDP

↓

MAC
```

---

# NDP Is Built On ICMPv6 ⭐⭐⭐⭐⭐

This is extremely important.

ARP:

```text
Standalone Protocol
```

NDP:

```text
Built inside ICMPv6
```

---

# Visualization

ARP:

```text
IPv4

↓

ARP
```

NDP:

```text
IPv6

↓

ICMPv6

↓

NDP
```

---

# Feature Comparison ⭐⭐⭐⭐⭐

| Feature | ARP | NDP |
|---------|-----|-----|
| IPv4 | ✅ | ❌ |
| IPv6 | ❌ | ✅ |
| Broadcast | ✅ | ❌ |
| Multicast | ❌ | ✅ |
| Router Discovery | ❌ | ✅ |
| Address Autoconfiguration | ❌ | ✅ |
| Duplicate Detection | ❌ | ✅ |
| Neighbor Discovery | ✅ | ✅ |

---

# Biggest Difference ⭐⭐⭐⭐⭐

ARP uses:

```text
Broadcast
```

NDP uses:

```text
Multicast
```

---

# Why Remove Broadcast?

Broadcast causes:

```text
Noise

Congestion

Inefficiency
```

IPv6 fixed this.

---

# Visual

ARP:

```text
One Device

↓

Everyone Receives
```

NDP:

```text
One Device

↓

Only Interested Devices
```

---

# ARP Workflow ⭐⭐⭐⭐⭐

```text
Know IP

↓

Need MAC

↓

Broadcast

↓

Reply

↓

Cache

↓

Communicate
```

---

# NDP Workflow ⭐⭐⭐⭐⭐

```text
Know IPv6

↓

Need MAC

↓

Multicast

↓

Reply

↓

Neighbor Cache

↓

Communicate
```

---

# ARP Request Visualization

```text
Laptop

↓

Who has

192.168.1.20 ?

↓

Switch

↙ ↓ ↓ ↓ ↘

Phone

TV

Printer

Camera
```

Everyone receives.

---

# NDP Visualization

```text
Laptop

↓

Neighbor Solicitation

↓

Multicast

↓

Specific Device

↓

Neighbor Advertisement

↓

Laptop
```

Far more efficient.

---

# NDP Does More Than ARP ⭐⭐⭐⭐⭐

NDP responsibilities:

```text
Neighbor Discovery

Router Discovery

Prefix Discovery

Address Autoconfiguration

Duplicate Address Detection

Neighbor Reachability
```

---

# NDP Toolbox Visualization

```text
NDP

↓

Neighbor Discovery

↓

Router Discovery

↓

Prefix Discovery

↓

Address Configuration

↓

Duplicate Detection
```

---

# Neighbor Solicitation (NS)

Equivalent to:

```text
ARP Request
```

Visualization:

```text
Who owns this IPv6?
```

---

# Neighbor Advertisement (NA)

Equivalent to:

```text
ARP Reply
```

Visualization:

```text
I own this IPv6.
```

---

# Router Solicitation (RS)

Host asks:

```text
Any router available?
```

---

# Router Advertisement (RA)

Router responds:

```text
I am here.

Use this prefix.
```

---

# SLAAC ⭐⭐⭐⭐⭐

Stateless Address Autoconfiguration.

Device automatically configures itself.

Visualization:

```text
Router

↓

Advertises Prefix

↓

Laptop

↓

Creates IPv6 Address
```

---

# Duplicate Address Detection (DAD) ⭐⭐⭐⭐⭐

IPv6 automatically checks.

Question:

```text
Is someone already using this address?
```

Very useful.

---

# Visualization

```text
New Device

↓

Check

↓

Already Exists?

↓

NO

↓

Use Address
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux supports both systems.

IPv4:

```text
ARP
```

IPv6:

```text
NDP
```

---

# Linux Internal Flow ⭐⭐⭐⭐⭐

IPv4:

```text
Application

↓

TCP

↓

IPv4

↓

ARP

↓

MAC

↓

NIC
```

---

IPv6:

```text
Application

↓

TCP

↓

IPv6

↓

NDP

↓

MAC

↓

NIC
```

---

# Linux Internal Architecture ⭐⭐⭐⭐⭐

```text
User Space

Application

↓

===================

Kernel Space

Socket

↓

TCP

↓

IPv4 / IPv6

↓

Neighbor Subsystem

↓

ARP / NDP

↓

NIC Driver

↓

NIC
```

---

# Linux Neighbor Subsystem ⭐⭐⭐⭐⭐

Linux unifies both.

Think:

```text
Neighbor Subsystem

↓

ARP

↓

NDP
```

---

# View Neighbor Table

```bash
ip neigh
```

or

```bash
ip -6 neigh
```

---

# Example

IPv4:

```text
192.168.1.1

↓

AA:BB:CC
```

IPv6:

```text
fe80::1

↓

AA:BB:CC
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

Docker supports both.

```text
IPv4

↓

ARP


IPv6

↓

NDP
```

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Modern clusters increasingly support:

```text
Dual Stack

↓

IPv4

+

IPv6
```

Both systems coexist.

---

# Cloud Perspective ⭐⭐⭐⭐⭐

AWS:

Supports both.

---

Azure:

Supports both.

---

GCP:

Supports both.

---

# Real Production Example

Modern architecture:

```text
User

↓

Load Balancer

↓

Application

↓

Database
```

May simultaneously run:

```text
IPv4

↓

ARP


IPv6

↓

NDP
```

---

# Troubleshooting ⭐⭐⭐⭐⭐

IPv4 issue:

```bash
ip neigh
```

---

IPv6 issue:

```bash
ip -6 neigh
```

---

Check routes:

```bash
ip route

ip -6 route
```

---

Check addresses:

```bash
ip addr

ip -6 addr
```

---

# Security Perspective ⭐⭐⭐⭐⭐

ARP attacks:

```text
ARP Spoofing

MITM
```

---

NDP attacks:

```text
Fake Router Advertisements

Neighbor Spoofing
```

Modern infrastructures use protections.

---

# Common Misconceptions

❌ IPv6 uses ARP.

Wrong.

---

❌ NDP only replaces ARP.

Wrong.

It replaces multiple systems.

---

❌ ARP and NDP are identical.

Wrong.

NDP is much larger.

---

❌ Broadcast exists in IPv6.

Wrong.

IPv6 uses multicast.

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
ARP

↓

IPv6 Version
```

Think:

```text
ARP

↓

Simple Tool


NDP

↓

Advanced Toolkit
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
IPv4

↓

ARP

↓

MAC


IPv6

↓

NDP

↓

MAC


ARP

↓

Broadcast


NDP

↓

Multicast


ARP

↓

Simple Discovery


NDP

↓

Full Network Discovery System
```

---

# WH Questions

## Why doesn't IPv6 use ARP?

IPv6 redesigned neighbor discovery.

---

## Why replace broadcast?

Efficiency.

---

## Why is NDP bigger?

It combines many functions.

---

## Why does Linux support both?

Dual-stack environments exist.

---

## Why do engineers care?

Modern infrastructures increasingly use IPv6.

---

# Key Takeaways

Remember forever.

```text
IPv4

↓

ARP


IPv6

↓

NDP


ARP

↓

Simple


NDP

↓

Advanced
```

---

# Recommended Reading Order

```text
arp.md

↓

arp-visuals.md

↓

arp-security.md

↓

arp-vs-ndp.md

↓

dns.md
```
