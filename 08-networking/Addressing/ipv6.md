# IPv6 (Internet Protocol Version 6)

> Learn why IPv6 was created, how Linux uses it internally, and why IPv6 is the future foundation of modern networking.

---

# Why Learn IPv6?

Imagine every object on Earth becoming connected.

Examples:

```text
Phones

Laptops

Cars

Smart TVs

Servers

Containers

Cloud Instances

Sensors

IoT Devices

Factories

Robots
```

Question:

```text
How do we give identities to all of them?
```

IPv4 started running out.

IPv6 solves this problem.

---

# What Problem Did IPv6 Solve?

IPv4 has:

```text
4.3 Billion addresses
```

Sounds large.

But reality:

```text
8 Billion Humans

×

Multiple Devices Each

×

Cloud Servers

×

IoT
```

IPv4 became insufficient.

---

# Why Not Just Increase IPv4?

Because IPv4 has architectural limitations.

Problems:

```text
Address exhaustion

Heavy NAT dependency

Routing complexity

Scalability limitations
```

A new protocol was created.

IPv6.

---

# Simple Definition

IPv6 is:

> A 128-bit addressing system designed to replace IPv4 and support the future growth of the internet.

---

# Big Picture

```text
Application

↓

TCP

↓

IPv6

↓

Router

↓

Internet

↓

Destination
```

---

# IPv6 Address Example

Example:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

Looks scary.

Don't memorize it.

Understand it.

---

# Structure

IPv6 contains:

```text
8 Groups
```

Each group:

```text
16 bits
```

Total:

```text
8 × 16

=

128 bits
```

---

# Visualization

```text
2001

0db8

85a3

0000

0000

8a2e

0370

7334
```

---

# Why Hexadecimal?

Imagine writing:

```text
128 bits
```

in binary.

Impossible for humans.

Hex is easier.

---

# Binary Visualization

```text
2001

↓

0010000000000001
```

Every block:

```text
16 bits
```

---

# Address Compression ⭐⭐⭐

IPv6 has shortcuts.

---

# Rule 1

Leading zeros may be removed.

Example:

```text
2001:0db8

↓

2001:db8
```

---

# Rule 2

One continuous block of zeros may become:

```text
::
```

Example:

```text
2001:db8:0:0:0:0:1

↓

2001:db8::1
```

---

# Important Rule

`::`

Can only be used once.

Bad:

```text
2001::db8::1
```

Invalid.

---

# Why Is IPv6 Huge?

IPv6 has:

```text
2^128
```

addresses.

Approximately:

```text
340 undecillion
```

Which is:

```text
340,282,366,920,938,463,463,374,607,431,768,211,456
```

Effectively unlimited for humans.

---

# IPv6 Philosophy Is Different

IPv4:

```text
Address conservation
```

IPv6:

```text
Address abundance
```

---

# IPv4 Mindset

```text
Save addresses
```

---

# IPv6 Mindset

```text
Give every device addresses freely
```

---

# IPv6 Hierarchy

Very important.

Structure:

```text
Global Prefix

↓

Subnet

↓

Interface ID
```

---

# Visualization

```text
2001:db8:abcd:1234

↓

Subnet

↓

Device
```

---

# Common IPv6 Types

---

# Global Unicast

Equivalent to public IP.

Internet routable.

Usually:

```text
2000::/3
```

---

# Link Local

Local communication only.

Starts with:

```text
fe80::
```

Every Linux interface automatically gets one.

---

# Unique Local Address (ULA)

Equivalent to private IP.

Starts with:

```text
fc00::

fd00::
```

---

# Loopback

Equivalent to:

```text
127.0.0.1
```

IPv6:

```text
::1
```

---

# Unspecified Address

Equivalent to:

```text
0.0.0.0
```

IPv6:

```text
::
```

---

# Broadcast Is Gone ⭐⭐⭐⭐⭐

This is extremely important.

IPv4:

```text
Broadcast
```

IPv6:

```text
No Broadcast
```

Instead:

```text
Multicast
```

is used.

---

# Why Remove Broadcast?

Broadcast causes:

```text
Noise

Congestion

Inefficiency
```

IPv6 improved this.

---

# Neighbor Discovery Protocol (NDP)

IPv4:

```text
ARP
```

IPv6:

```text
NDP
```

---

# Visualization

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

# Stateless Address Autoconfiguration (SLAAC)

This is one of IPv6's biggest features.

Device joins network.

Automatically configures itself.

Visualization:

```text
Router

↓

Advertise Network

↓

Laptop

↓

Creates Address
```

---

# End-To-End Example

Suppose:

```text
curl google.com
```

Flow:

```text
Application

↓

TCP

↓

IPv6

↓

Router

↓

Internet

↓

Google
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux fully supports IPv6.

Most Linux systems already have IPv6 enabled.

---

# View IPv6 Addresses

```bash
ip -6 addr
```

---

# View IPv6 Routes

```bash
ip -6 route
```

---

# View Interfaces

```bash
ip link
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```text
curl google.com
```

Linux internally performs:

```text
Application

↓

Socket

↓

TCP

↓

IPv6

↓

Routing Table

↓

Neighbor Discovery

↓

NIC

↓

Internet
```

---

# Internal Linux Visualization

```text
User Space

Application

↓

=================

Kernel Space

Socket

↓

TCP

↓

IPv6

↓

NDP

↓

Driver

↓

NIC
```

---

# Linux Usually Has Multiple IPv6 Addresses

One interface may have:

```text
Link Local

+

Global

+

Temporary
```

Example:

```text
eth0

↓

fe80::

↓

2001::

↓

Temporary Address
```

---

# Docker Perspective

Docker supports IPv6.

Container:

```text
Container

↓

Bridge

↓

IPv6 Network
```

---

# Kubernetes Perspective

Modern Kubernetes increasingly supports dual stack.

Example:

```text
IPv4

+

IPv6
```

simultaneously.

---

# Cloud Perspective

AWS:

Supports IPv6.

---

Azure:

Supports IPv6.

---

GCP:

Supports IPv6.

---

# Real Production Example

Modern architecture:

```text
Internet

↓

Load Balancer

↓

Application

↓

Database
```

Every layer may use IPv6.

---

# Security Perspective

Advantages:

```text
No NAT dependency

Better end-to-end communication

IPsec support
```

But:

Larger address spaces make scanning harder.

---

# Troubleshooting

Problem:

```text
Website works on IPv4

Fails on IPv6
```

Check:

```bash
ip -6 addr
```

---

Check routes:

```bash
ip -6 route
```

---

Check connectivity:

```bash
ping6 google.com
```

or

```bash
ping -6 google.com
```

---

# Common Misconceptions

❌ IPv6 is only for the future

Wrong.

It's already here.

---

❌ IPv6 replaced IPv4

Wrong.

Currently:

```text
IPv4

+

IPv6
```

coexist.

---

❌ IPv6 is harder

Wrong.

It only looks bigger.

---

❌ NAT is mandatory

Wrong.

IPv6 was designed to reduce NAT dependence.

---

# Engineer Mental Model

Never think:

```text
IPv4 2.0
```

Think:

```text
New Internet Design
```

---

# Visual Summary

```text
IPv4

↓

Address Conservation


IPv6

↓

Address Abundance


IPv4

↓

ARP


IPv6

↓

NDP


IPv4

↓

Broadcast


IPv6

↓

Multicast
```

---

# IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Size | 32-bit | 128-bit |
| Total Addresses | 4.3 Billion | 340 Undecillion |
| NAT | Common | Reduced Need |
| Broadcast | Yes | No |
| ARP | Yes | No |
| NDP | No | Yes |

---

# WH Questions

## What is IPv6?

128-bit addressing system.

---

## Why was IPv6 created?

IPv4 exhaustion.

---

## Why remove broadcast?

Efficiency.

---

## Why replace ARP?

Scalability.

---

## Why do Linux engineers care?

Modern infrastructure increasingly uses IPv6.

---

# Key Takeaways

Remember forever.

```text
IPv4

↓

Limited


IPv6

↓

Massively Scalable


IPv4

↓

Address Conservation


IPv6

↓

Address Abundance
```

---

# What's Next?

Now we enter:

```text
# Core protocols

arp.md
```

This is where networking starts becoming extremely interesting because you'll finally understand:

> How Linux converts IP addresses into MAC addresses.
