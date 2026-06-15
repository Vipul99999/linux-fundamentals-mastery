# MAC Address (Media Access Control Address)

> Learn how devices identify each other inside local networks, why IP addresses alone are not enough, and how Linux, switches, and modern infrastructures use MAC addresses.

---

# Why Learn MAC Addresses?

One of the biggest beginner questions is:

```text
If IP addresses already exist...

Why do MAC addresses exist?
```

This is one of the most important networking questions.

The answer is:

```text
IP

↓

Global Navigation


MAC

↓

Local Delivery
```

They solve different problems.

Without MAC addresses:

- Local communication breaks
- Ethernet cannot work
- WiFi cannot work
- ARP cannot work
- Linux networking cannot function properly

---

# The Big Question

Imagine:

```text
Laptop

192.168.1.10
```

wants to communicate with:

```text
192.168.1.20
```

Question:

```text
How do we physically find the device?
```

IP alone cannot do this.

MAC solves it.

---

# Simple Definition

A MAC address is:

> A unique hardware identifier assigned to a network interface for communication inside local networks.

---

# Mental Model

Think:

```text
IP

↓

Where should we go?


MAC

↓

Who physically receives this?
```

---

# House Delivery Analogy

Imagine postal delivery.

IP Address:

```text
Street Address
```

MAC Address:

```text
Specific House Door
```

---

# Big Picture

```text
Application

↓

TCP

↓

IP

↓

MAC

↓

Network Cable / WiFi

↓

Destination
```

---

# Why Do We Need Two Addresses?

Networking has two different problems.

---

# Problem 1

```text
Where should data go globally?
```

Answer:

```text
IP
```

---

# Problem 2

```text
Who physically receives it locally?
```

Answer:

```text
MAC
```

---

# Visualization

```text
Laptop

↓

IP

↓

Router

↓

MAC

↓

Device
```

---

# What Does A MAC Address Look Like?

Example:

```text
A4:5E:60:12:34:56
```

or

```text
A4-5E-60-12-34-56
```

---

# Structure

MAC address:

```text
48 bits
```

equals:

```text
6 bytes
```

---

# Visualization

```text
A4

5E

60

12

34

56
```

---

# Binary Visualization

```text
10100100

01011110

01100000

00010010

00110100

01010110
```

---

# MAC Address Structure

Two parts exist.

```text
OUI

+

Device Identifier
```

---

# OUI (Organizationally Unique Identifier)

First 24 bits.

Identifies manufacturer.

Examples:

```text
Intel

Dell

Apple

Samsung
```

---

# Device Identifier

Last 24 bits.

Unique per device.

---

# Visualization

```text
A4:5E:60 | 12:34:56

Manufacturer | Device
```

---

# Is MAC Truly Unique?

Usually yes.

But modern systems can spoof MAC addresses.

So:

```text
Unique by design

↓

Not guaranteed forever
```

---

# Network Interface Cards (NICs)

MAC addresses belong to interfaces.

Examples:

```text
Ethernet

WiFi

Virtual Interfaces

Docker Interfaces
```

Each interface gets its own MAC.

---

# Example

Laptop:

```text
WiFi

MAC A
```

Ethernet:

```text
MAC B
```

---

# Communication Flow

Suppose:

```text
Laptop

192.168.1.10
```

wants:

```text
192.168.1.20
```

Flow:

```text
Laptop

↓

ARP

↓

Destination MAC

↓

Ethernet Frame

↓

Destination
```

---

# Ethernet Uses MAC

Ethernet frames contain:

```text
Source MAC

Destination MAC
```

Visualization:

```text
┌───────────────────┐
│ Ethernet Header   │
├───────────────────┤
│ IP Header         │
├───────────────────┤
│ TCP Header        │
├───────────────────┤
│ Application Data  │
└───────────────────┘
```

---

# Source MAC

Who sent this?

Example:

```text
A4:5E:60:12:34:56
```

---

# Destination MAC

Who should physically receive this?

Example:

```text
C2:10:44:AB:90:FF
```

---

# Why MAC Is Local Only

Routers replace MAC addresses.

IP stays.

Visualization:

```text
Laptop

↓

Router

↓

Google
```

---

# First Network

```text
Source MAC

↓

Router MAC
```

---

# Second Network

```text
Router MAC

↓

Next Device MAC
```

---

# IP vs MAC Journey

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

IP:

```text
Remains

192.168.1.10

↓

142.x.x.x
```

MAC:

```text
Changes

At every hop
```

---

# Visualization

```text
IP

━━━━━━━━━━━━━━━>

Entire Journey


MAC

→

→

→

Changes
```

---

# Why Does ARP Exist?

Question:

```text
I know the IP.

How do I find the MAC?
```

ARP solves this.

---

# ARP Visualization

```text
192.168.1.20

↓

Who owns this?

↓

MAC Returned

↓

Send Data
```

---

# Linux Perspective ⭐⭐⭐

Linux associates MAC addresses with interfaces.

Example:

```text
eth0

MAC Address
```

---

# View Interfaces

```bash
ip link
```

Example:

```text
2: eth0

link/ether a4:5e:60:12:34:56
```

---

# Alternative

```bash
ip addr
```

---

# Old Command

```bash
ifconfig
```

---

# Linux Networking Internals ⭐⭐⭐

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

IP

↓

Routing

↓

ARP

↓

MAC Discovery

↓

Ethernet Frame

↓

NIC

↓

Internet
```

---

# Internal Linux Visualization

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

ARP

↓

MAC

↓

NIC Driver

↓

NIC Hardware
```

---

# ARP Cache

Linux remembers MAC addresses.

---

# View Cache

```bash
ip neigh
```

Example:

```text
192.168.1.1

lladdr

AA:BB:CC:DD:EE:FF
```

---

# Why Cache Exists?

Avoid repeatedly asking.

---

# Visualization

Without cache:

```text
ARP

↓

ARP

↓

ARP

↓

ARP
```

Too expensive.

---

# Switch Perspective ⭐⭐⭐

Switches operate using MAC addresses.

Switches do NOT care about:

```text
TCP

HTTP

DNS
```

Switches only care about:

```text
MAC addresses
```

---

# Switch Table

```text
MAC

↓

Port
```

Example:

```text
A4:5E:60:12:34:56

↓

Port 1
```

---

# MAC Learning Visualization

```text
Frame Arrives

↓

Read Source MAC

↓

Store MAC

↓

Associate Port
```

---

# Modern Infrastructure Usage

---

# Home Networks

```text
Laptop

↓

Router

↓

Phone
```

Uses MAC heavily.

---

# Cloud

Cloud often virtualizes MAC addresses.

---

# Docker

Each container interface gets a MAC.

---

# Kubernetes

Pods receive virtual interfaces.

These interfaces have MAC addresses.

---

# Real Production Example

Opening:

```text
https://github.com
```

Journey:

```text
Browser

↓

DNS

↓

TCP

↓

IP

↓

ARP

↓

MAC

↓

Ethernet

↓

Router

↓

Internet
```

---

# Common Problems

---

# Duplicate MAC

Rare but possible.

Symptoms:

```text
Random network failures
```

---

# ARP Problems

Symptoms:

```text
Cannot communicate locally
```

---

# Wrong Switch Learning

Symptoms:

```text
Intermittent failures
```

---

# Troubleshooting Commands

---

# View interfaces

```bash
ip link
```

---

# View ARP cache

```bash
ip neigh
```

---

# Show detailed info

```bash
ip -details link
```

---

# Security Considerations ⭐⭐⭐

Attackers may exploit:

```text
MAC spoofing

ARP spoofing

Man In The Middle
```

Never trust MAC addresses alone.

---

# Common Misconceptions

❌ MAC = Internet identity

Wrong.

---

❌ MAC never changes

Wrong.

It can be spoofed.

---

❌ MAC works globally

Wrong.

MAC is local.

---

# Engineer Mental Model

Always think:

```text
IP

↓

Where?


MAC

↓

Who locally?
```

---

# Visual Summary

```text
Application

↓

TCP

↓

IP

↓

ARP

↓

MAC

↓

Ethernet

↓

Destination
```

---

# WH Questions

## What is a MAC address?

Hardware interface identity.

---

## Why do MAC addresses exist?

Local delivery.

---

## Why isn't IP enough?

IP does not identify local hardware.

---

## Why does ARP exist?

To map IP to MAC.

---

## Why do switches use MAC?

To forward frames.

---

## Why does Linux care?

Linux networking depends on MAC discovery.

---

# Key Takeaways

Remember forever:

```text
IP

↓

Global Navigation


MAC

↓

Local Delivery
```

---

# What's Next?

```text
subnetting.md
```

You will learn one of the most important engineering skills in networking.

This is where networking starts becoming very practical.
