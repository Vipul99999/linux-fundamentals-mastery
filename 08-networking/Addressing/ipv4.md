# IPv4 (Internet Protocol Version 4)

> Learn how billions of devices are identified, how Linux uses IPv4 internally, and why IPv4 still powers a huge portion of modern infrastructure.

---

# Why Learn IPv4?

Imagine a world with billions of devices.

Examples:

```text
Laptops

Phones

Servers

Routers

Containers

Virtual Machines

IoT Devices

Cloud Instances
```

Question:

```text
How do we uniquely identify all of them?
```

IPv4 solves this.

---

# What Problem Does IPv4 Solve?

Networking needs answers for three questions.

```text
Who am I?

↓

Who are you?

↓

How do I reach you?
```

IPv4 solves all three.

---

# What Is IPv4?

IPv4 means:

```text
Internet Protocol Version 4
```

It is a protocol responsible for:

```text
Logical Addressing

Packet Delivery

Routing
```

---

# Simple Definition

IPv4 is:

> A 32-bit logical addressing system used to identify devices on networks.

---

# Why Is It Called IPv4?

There were earlier versions.

IPv4 became the standard internet protocol.

Today:

```text
IPv4

+

IPv6
```

coexist.

---

# Big Picture

```text
Application

↓

TCP

↓

IPv4

↓

Router

↓

Internet

↓

Destination
```

---

# IPv4 Address Format

Example:

```text
192.168.1.10
```

It has:

```text
4 sections
```

called:

```text
Octets
```

---

# Visualization

```text
192 . 168 . 1 . 10

↓

8bit 8bit 8bit 8bit
```

---

# Why 4 Numbers?

Because:

```text
8 + 8 + 8 + 8

=

32 bits
```

---

# Binary Visualization

```text
192.168.1.10

↓

11000000

10101000

00000001

00001010
```

---

# Each Octet Range

Every octet:

```text
0 → 255
```

Because:

```text
8 bits

↓

2^8

↓

256 values
```

---

# Valid Examples

```text
10.0.0.1

172.16.0.1

192.168.1.1

8.8.8.8
```

---

# Invalid Examples

```text
256.1.1.1

192.168.1.999

300.0.0.1
```

---

# IPv4 Structure

Every IPv4 address has two components.

```text
Network Portion

+

Host Portion
```

---

# Visualization

```text
192.168.1.25

↓

192.168.1 | 25

↓

Network | Host
```

---

# Why Split Addresses?

Network answers:

```text
Which network?
```

Host answers:

```text
Which device?
```

---

# House Analogy

```text
Street

↓

House Number
```

Networking:

```text
Network

↓

Host
```

---

# Why Not Use One Giant List?

Imagine:

```text
8 Billion Devices
```

without grouping.

Impossible.

Hierarchy is required.

---

# IPv4 Is Hierarchical

```text
Internet

↓

ISP

↓

Organization

↓

Department

↓

Network

↓

Device
```

This scales globally.

---

# Theoretical Address Space

IPv4 has:

```text
2^32
```

addresses.

---

# Total

```text
4,294,967,296
```

addresses.

About:

```text
4.3 Billion
```

---

# Why Did IPv4 Become A Problem?

Modern world:

```text
Phones

Laptops

Cloud

IoT

Containers

Smart TVs

Cars
```

Devices exploded.

IPv4 began running out.

---

# This Led To IPv6

But IPv4 is still heavily used today.

---

# How IPv4 Moves Data

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

IPv4

↓

Router

↓

Internet

↓

Google
```

---

# IPv4 Header

IPv4 adds instructions.

Packet structure:

```text
┌────────────────────┐
│ IPv4 Header        │
├────────────────────┤
│ TCP Header         │
├────────────────────┤
│ Application Data   │
└────────────────────┘
```

---

# Important IPv4 Header Fields

```text
Source IP

Destination IP

TTL

Protocol

Header Length

Fragmentation Info
```

---

# Source IP

Who sent this?

Example:

```text
192.168.1.10
```

---

# Destination IP

Who should receive this?

Example:

```text
142.x.x.x
```

---

# TTL (Time To Live)

TTL prevents infinite loops.

Example:

```text
64
```

Every router:

```text
TTL - 1
```

Eventually:

```text
0

↓

Discard packet
```

---

# Visualization

```text
64

↓

63

↓

62

↓

61
```

---

# Protocol Field

Tells IP what sits above it.

Examples:

```text
6 = TCP

17 = UDP

1 = ICMP
```

---

# Fragmentation

Suppose:

```text
3000 bytes
```

Network MTU:

```text
1500 bytes
```

Packet splits.

Visualization:

```text
3000

↓

1500

↓

1500
```

---

# Linux Perspective ⭐⭐⭐

Linux networking revolves around IPv4.

Linux assigns IPs to interfaces.

Example:

```text
eth0

192.168.1.10
```

---

# View IP Addresses

```bash
ip addr
```

Example:

```text
2: eth0

inet 192.168.1.10/24
```

---

# Add Temporary Address

```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

---

# Remove Address

```bash
sudo ip addr del 192.168.1.50/24 dev eth0
```

---

# Linux Networking Internals ⭐⭐⭐

Linux internally performs:

```text
Application

↓

Socket API

↓

TCP

↓

IPv4

↓

Routing Table

↓

ARP

↓

Interface

↓

NIC

↓

Internet
```

---

# Internal Packet Journey

Suppose:

```text
curl github.com
```

Linux performs:

```text
curl

↓

DNS

↓

Socket

↓

TCP

↓

IPv4

↓

Routing Table

↓

ARP

↓

eth0

↓

NIC

↓

Internet
```

---

# Linux Route Selection

Linux asks:

```text
Destination IP?

↓

Routing Table

↓

Which Interface?

↓

Transmit
```

---

# View Routing Table

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
```

---

# Real World Usage

---

# Home Network

```text
Laptop

↓

Router

↓

Internet
```

---

# Datacenter

```text
Servers

↓

Switches

↓

Routers
```

---

# Cloud

```text
VM

↓

Virtual Network

↓

Internet
```

---

# Docker

```text
Container

↓

Bridge

↓

Host
```

---

# Kubernetes

```text
Pod

↓

Service

↓

Internet
```

---

# Production Example

Open:

```text
https://github.com
```

Journey:

```text
Browser

↓

DNS

↓

GitHub IP

↓

TCP

↓

IPv4

↓

Internet

↓

GitHub
```

---

# Common Problems

---

# Duplicate IP

Two devices:

```text
192.168.1.10

192.168.1.10
```

Symptoms:

```text
Intermittent failures

Disconnects
```

---

# Wrong Gateway

Symptoms:

```text
Local network works

Internet fails
```

---

# Wrong Route

Symptoms:

```text
Packets disappear
```

---

# Troubleshooting Commands

---

# View IP

```bash
ip addr
```

---

# View route

```bash
ip route
```

---

# Test connectivity

```bash
ping
```

---

# Trace route

```bash
traceroute
```

---

# Security Considerations

Attackers may abuse IPv4.

Examples:

```text
IP Spoofing

Reconnaissance

Scanning

DDoS
```

---

# Engineer Mental Model

Never think:

```text
Laptop

↓

Server
```

Think:

```text
Application

↓

TCP

↓

IPv4

↓

Router

↓

Internet

↓

Destination
```

---

# Visual Summary

```text
WHO AM I?

↓

Source IP


WHO ARE YOU?

↓

Destination IP


HOW DO I REACH YOU?

↓

Routing
```

---

# WH Questions

## What is IPv4?

A 32-bit logical addressing system.

---

## Why does IPv4 exist?

To identify devices.

---

## Why is IPv4 hierarchical?

To scale globally.

---

## Why do routers care about IPv4?

Routers use destination IPs.

---

## Why does Linux care?

Linux networking is built around IP.

---

## Why is IPv4 running out?

There are more devices than available addresses.

---

# Key Takeaways

Remember this forever.

```text
Identity

↓

Source

↓

Destination

↓

Routing

↓

Communication
```

---

# What's Next?

```text
private-vs-public-ip.md
```

Then:

```text
mac-address.md
```

Then:

```text
subnetting.md

cidr.md
```

Finally:

```text
ipv6.md
```
