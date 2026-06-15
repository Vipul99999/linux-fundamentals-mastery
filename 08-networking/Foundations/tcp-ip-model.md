# TCP/IP Model

> Learn the actual networking model Linux, the internet, cloud platforms, containers, and modern infrastructure use every second.

---

# Why Learn TCP/IP?

If OSI is the theory…

TCP/IP is reality.

Almost every modern system uses TCP/IP.

Examples:

- Linux
- Windows
- macOS
- Android
- iOS
- AWS
- Azure
- GCP
- Docker
- Kubernetes
- The Internet itself

If networking is a language…

TCP/IP is the language everyone agreed to speak.

---

# What Problem Did TCP/IP Solve?

Imagine three networks.

```text
University Network

Military Network

Research Network
```

Problem:

```text
They cannot communicate.
```

Each network speaks differently.

A universal communication standard was needed.

TCP/IP solved this.

---

# Simple Definition

TCP/IP is a suite of protocols that allows different networks to communicate with each other.

---

# Why Is It Called TCP/IP?

Two major protocols became foundational.

```text
TCP

Transmission Control Protocol

+

IP

Internet Protocol
```

Eventually, the entire communication architecture became known as:

```text
TCP/IP Model
```

---

# Core Philosophy

Instead of one giant communication system:

Break responsibilities into layers.

```text
Application

↓

Transport

↓

Internet

↓

Network Access
```

---

# Four Layers

```text
4. Application

3. Transport

2. Internet

1. Network Access
```

---

# Big Picture

```text
Application

↓

Transport

↓

Internet

↓

Network Access

↓

Physical Network

↓

Destination
```

---

# Layer 4: Application Layer

## Purpose

Provides services to users and applications.

---

# Examples

Applications:

```text
Browser

SSH Client

Email Client

curl

wget
```

Protocols:

```text
HTTP

HTTPS

DNS

SSH

SMTP

FTP
```

---

# Example

```text
Browser

↓

google.com
```

---

# Responsibilities

```text
Data generation

Service requests

User interaction
```

---

# Linux Examples

```bash
curl

wget

ssh

scp

dig
```

---

# Layer 3: Transport Layer

## Purpose

Reliable end-to-end communication.

---

# Main Protocols

```text
TCP

UDP
```

---

# TCP

Reliable.

Features:

```text
Ordering

Acknowledgements

Error recovery

Retransmission
```

Examples:

```text
HTTPS

SSH

Database Connections
```

---

# UDP

Fast.

Features:

```text
No acknowledgement

No retransmission

No ordering
```

Examples:

```text
Streaming

Gaming

Video Calls

DNS
```

---

# Layer 2: Internet Layer

## Purpose

Moves packets between networks.

---

# Protocols

```text
IP

ICMP

ARP
```

Responsibilities:

```text
Addressing

Routing

Packet forwarding
```

---

# Layer 1: Network Access Layer

## Purpose

Handles physical network communication.

---

# Examples

```text
Ethernet

WiFi

Fiber

NIC
```

Responsibilities:

```text
Frame creation

Signal transmission

Local communication
```

---

# Complete Visual

```text
┌─────────────────────┐
│ Application Layer   │
├─────────────────────┤
│ Transport Layer     │
├─────────────────────┤
│ Internet Layer      │
├─────────────────────┤
│ Network Access      │
└─────────────────────┘
```

---

# The Internet Is A Giant TCP/IP System

```text
Your Device

↓

Router

↓

ISP

↓

Internet

↓

Google
```

Everything speaks TCP/IP.

---

# End To End Example

Suppose you visit:

```text
https://google.com
```

---

# Step 1

Application Layer

```text
Browser

↓

Generate HTTP Request
```

---

# Step 2

Transport Layer

```text
TCP

↓

Split data

↓

Add port numbers
```

---

# Step 3

Internet Layer

```text
IP

↓

Add addresses

↓

Find route
```

---

# Step 4

Network Access Layer

```text
Ethernet

↓

Frame creation

↓

Physical transmission
```

---

# Data Journey Visualization

```text
Browser

↓

HTTP

↓

TCP

↓

IP

↓

Ethernet

↓

WiFi/Cable

↓

Internet

↓

Google Server
```

---

# Encapsulation

Each layer adds information.

---

# Before

```text
Hello Google
```

---

# Application Layer

```text
[HTTP]

Hello Google
```

---

# Transport Layer

```text
[TCP]

[HTTP]

Hello Google
```

---

# Internet Layer

```text
[IP]

[TCP]

[HTTP]

Hello Google
```

---

# Network Access Layer

```text
[Ethernet]

[IP]

[TCP]

[HTTP]

Hello Google
```

---

# Decapsulation

Destination removes layers.

```text
Ethernet

↓

IP

↓

TCP

↓

HTTP

↓

Application
```

---

# Linux Networking Internals ⭐

This is the most important section.

Linux internally does something like:

```text
Application

↓

Socket API

↓

Kernel Space

↓

TCP/IP Stack

↓

Routing Subsystem

↓

Traffic Control

↓

Firewall

↓

Network Interface

↓

NIC Driver

↓

NIC Hardware

↓

Cable/WiFi
```

---

# Linux Network Stack Visualization

```text
User Space

Application

↓

System Calls

================================

Kernel Space

Socket Layer

↓

TCP/UDP

↓

IP

↓

ARP

↓

Network Driver

↓

NIC Hardware
```

---

# User Space vs Kernel Space

Applications never directly access hardware.

Applications say:

```text
Send this data.
```

Kernel does the rest.

---

# Internal Flow

```text
Browser

↓

Socket

↓

Kernel

↓

TCP

↓

IP

↓

ARP

↓

NIC Driver

↓

NIC

↓

Internet
```

---

# Linux Components Involved

| Component | Responsibility |
|-----------|---------------|
| Socket API | Application communication |
| TCP | Reliability |
| UDP | Speed |
| IP | Addressing |
| Routing Table | Path selection |
| ARP | Find MAC addresses |
| NIC Driver | Hardware communication |
| NIC | Physical transmission |

---

# Real Production Example

Suppose:

```text
https://github.com
```

Your machine does:

```text
Browser

↓

DNS

↓

TCP Handshake

↓

TLS Handshake

↓

HTTP Request

↓

Routing

↓

Internet

↓

GitHub
```

Millions of times every second globally.

---

# Modern Infrastructure Built On TCP/IP

## Cloud

```text
Users

↓

Load Balancer

↓

Servers

↓

Databases
```

---

## Docker

```text
Container

↓

Virtual Ethernet

↓

Bridge

↓

Host Network

↓

Internet
```

---

## Kubernetes

```text
Pod

↓

Service

↓

Ingress

↓

Internet
```

---

# Troubleshooting Using TCP/IP Layers

---

# Application Layer

Problems:

```text
API failure

DNS issue

Application crash
```

Tools:

```bash
curl

dig
```

---

# Transport Layer

Problems:

```text
Port closed

TCP timeout

UDP loss
```

Tools:

```bash
ss

netstat
```

---

# Internet Layer

Problems:

```text
Wrong IP

Routing issue

Gateway issue
```

Tools:

```bash
ip route

ping

traceroute
```

---

# Network Access Layer

Problems:

```text
Cable unplugged

NIC failure

WiFi issue
```

Tools:

```bash
ip link

ethtool
```

---

# OSI vs TCP/IP Quick Comparison

| OSI | TCP/IP |
|-----|--------|
| 7 Layers | 4 Layers |
| Educational | Practical |
| Conceptual | Implemented |
| Reference Model | Actual Architecture |

---

# WH Questions

## What is TCP/IP?

A communication architecture.

---

## Why was TCP/IP created?

To connect different networks.

---

## Why is TCP/IP important?

The internet uses it.

---

## Why does Linux use TCP/IP?

Linux implements it natively.

---

## Why do layers exist?

To separate responsibilities.

---

## Why is troubleshooting easier?

Problems can be isolated.

---

# Key Takeaways

Always think:

```text
Application

↓

Transport

↓

Internet

↓

Network Access

↓

Destination
```

---

# What's Next?

```text
osi-vs-tcpip.md ⭐ (recommended additional file)
```

Then proceed to:

```text
encapsulation-decapsulation.md
```

because that is where networking truly starts becoming intuitive.
