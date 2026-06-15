# IP Addressing

> Learn how devices identify each other, how packets find destinations, and how Linux uses IP addresses to communicate across networks.

---

# Why Learn IP Addressing?

Imagine a world without addresses.

You write a letter.

But there is no:

- House number
- Street name
- City
- Country

Could the post office deliver it?

No.

Networking has the same problem.

Devices need identities.

IP addressing solves this problem.

---

# What Problem Does IP Addressing Solve?

Networking must answer three questions.

```text
Who am I?

↓

Who are you?

↓

How do I reach you?
```

IP addressing solves all three.

---

# Simple Definition

An IP address is:

> A logical address assigned to a device so that it can be identified and communicate on a network.

---

# Big Picture

```text
Device

↓

IP Address

↓

Router

↓

Internet

↓

Destination
```

---

# Real World Analogy

Imagine postal delivery.

```text
Person

↓

House

↓

Street

↓

City

↓

Country
```

Networking equivalent:

```text
Application

↓

Device

↓

IP Address

↓

Router

↓

Internet
```

---

# What Is "IP"?

IP means:

```text
Internet Protocol
```

Its responsibilities are:

```text
Identify devices

↓

Move packets

↓

Find destinations
```

---

# IP Is Logical, Not Physical

Many beginners confuse this.

IP address:

```text
Can change
```

MAC address:

```text
Usually stays fixed
```

---

# IP vs MAC

| Feature | IP Address | MAC Address |
|---------|------------|-------------|
| Type | Logical | Physical |
| Layer | Network Layer | Data Link Layer |
| Can Change | Yes | Usually No |
| Used By | Routers | Switches |
| Scope | Entire Network | Local Network |

---

# How Does Communication Happen?

Suppose:

```text
Laptop

192.168.1.10
```

wants to access:

```text
google.com

142.x.x.x
```

Flow:

```text
Laptop

↓

Router

↓

ISP

↓

Internet

↓

Google
```

IP addresses make this possible.

---

# Every IP Has Two Parts

This is extremely important.

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

# Why Separate Them?

Network:

```text
Where should I go?
```

Host:

```text
Which device?
```

---

# House Analogy

```text
Street

↓

House
```

Networking:

```text
Network

↓

Host
```

---

# Devices Need Unique Addresses

Inside the same network:

```text
192.168.1.10

192.168.1.11

192.168.1.12
```

Good.

---

This is bad:

```text
192.168.1.10

192.168.1.10
```

Duplicate IPs create conflicts.

---

# End-To-End Communication

Suppose:

```text
Your Laptop

↓

Google
```

Visualization:

```text
192.168.1.10

↓

192.168.1.1

↓

ISP

↓

Internet

↓

142.x.x.x
```

---

# IP Address Hierarchy

The internet is hierarchical.

```text
Internet

↓

Countries

↓

ISPs

↓

Organizations

↓

Networks

↓

Devices
```

This allows scaling.

---

# Why Hierarchy Exists

Imagine:

```text
8 billion devices

↓

One flat list
```

Impossible.

Hierarchy organizes everything.

---

# Packet Journey Visualization

```text
Application

↓

TCP

↓

IP Address Added

↓

Router

↓

Internet

↓

Destination
```

---

# What Information Does IP Add?

IP adds:

```text
Source IP

Destination IP

TTL

Protocol
```

---

# Packet Visualization

```text
┌────────────────────┐
│ IP Header          │
├────────────────────┤
│ TCP Header         │
├────────────────────┤
│ Application Data   │
└────────────────────┘
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
TTL = 64
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

# TTL Visualization

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

# Router Perspective

Routers only care about:

```text
Destination IP
```

Router does NOT care about:

```text
HTML

Images

Videos
```

It only cares:

```text
Where should this packet go?
```

---

# Linux Perspective ⭐⭐⭐

Linux assigns IP addresses to interfaces.

Example:

```text
eth0

192.168.1.10
```

---

# View Interfaces

```bash
ip addr
```

Example:

```text
2: eth0

inet 192.168.1.10/24
```

---

# View Routing

```bash
ip route
```

Example:

```text
default via 192.168.1.1
```

---

# Linux Internals ⭐⭐⭐

Linux internally performs:

```text
Application

↓

Socket API

↓

TCP

↓

IP

↓

Routing Table

↓

Network Interface

↓

NIC

↓

Internet
```

---

# Internal Linux Flow

Suppose:

```text
curl google.com
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

IP

↓

Routing Table

↓

eth0

↓

NIC

↓

Internet
```

---

# How Linux Chooses A Route

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

# Routing Visualization

```text
Packet

↓

Destination IP

↓

Routing Table

↓

eth0

↓

Internet
```

---

# Modern Infrastructure Usage

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

↓

Internet
```

---

# Kubernetes

```text
Pod

↓

Service

↓

Node

↓

Internet
```

---

# Real Production Example

Opening:

```text
github.com
```

Flow:

```text
Browser

↓

DNS

↓

GitHub IP

↓

TCP

↓

IP

↓

Internet

↓

GitHub
```

---

# Troubleshooting

---

# Problem 1

Cannot access internet.

Check:

```bash
ip addr
```

Does interface have an IP?

---

# Problem 2

Wrong route.

Check:

```bash
ip route
```

---

# Problem 3

Duplicate IP.

Symptoms:

```text
Intermittent connectivity

Random disconnects
```

---

# Problem 4

Wrong gateway.

Symptoms:

```text
Local network works

Internet fails
```

---

# Troubleshooting Flow

```text
No Internet

↓

IP Exists?

↓

YES

↓

Route Exists?

↓

YES

↓

Gateway Exists?

↓

YES

↓

DNS Working?

↓

YES

↓

Destination Reachable?
```

---

# Security Considerations

Attackers may exploit:

```text
IP spoofing

Scanning

Reconnaissance
```

Tools:

```text
nmap

masscan
```

---

# Engineer Mental Model

Never think:

```text
Laptop

↓

Google
```

Think:

```text
Application

↓

TCP

↓

IP

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

## What is an IP address?

A logical network identity.

---

## Why do devices need IP addresses?

To communicate.

---

## Why is IP logical?

It can change.

---

## Why do routers need IP addresses?

To forward packets.

---

## Why does Linux care about IP?

Linux networking is built around IP.

---

## Why do duplicate IPs fail?

Two devices cannot share the same identity.

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
ipv4.md
```

You will learn:

```text
32-bit addresses

Address classes

CIDR

Reserved ranges

Loopback

Private ranges

Broadcast addresses

Linux internals

Real-world usage
```
