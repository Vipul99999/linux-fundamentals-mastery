# OSI Model (Open Systems Interconnection)

> Learn the universal networking mental model used to understand how systems communicate.

---

# Why Learn The OSI Model?

The OSI model is NOT a protocol.

The OSI model is NOT software.

The OSI model is a way of thinking.

It is a framework that divides networking into layers.

Without layers, networking would become extremely difficult.

---

# What Problem Did The OSI Model Solve?

Imagine building networking without organization.

Everything would be mixed together.

```text
Application Code

DNS

Encryption

Sessions

TCP

IP

MAC

Electrical Signals
```

It would become impossible to manage.

The OSI model solved this by separating responsibilities.

---

# Simple Definition

The OSI model divides networking into 7 layers.

Each layer has one responsibility.

```text
7 Application

6 Presentation

5 Session

4 Transport

3 Network

2 Data Link

1 Physical
```

---

# Core Idea

Each layer:

```text
Receives data

↓

Processes data

↓

Adds its own information

↓

Passes data downward
```

At the destination:

```text
Receives data

↓

Removes information

↓

Passes data upward
```

---

# Why Layers Exist

Layers provide:

```text
Separation of responsibilities

↓

Modularity

↓

Maintainability

↓

Scalability

↓

Standardization

↓

Interoperability
```

---

# Big Picture Visualization

```text
Application

↓

Presentation

↓

Session

↓

Transport

↓

Network

↓

Data Link

↓

Physical

↓

Network Cable / WiFi

↓

Destination

↓

Physical

↓

Data Link

↓

Network

↓

Transport

↓

Session

↓

Presentation

↓

Application
```

---

# The Seven Layers

| Layer | Name | Responsibility |
|------|------|----------------|
| 7 | Application | User interaction |
| 6 | Presentation | Data formatting |
| 5 | Session | Connection management |
| 4 | Transport | Reliable communication |
| 3 | Network | Path selection |
| 2 | Data Link | Local communication |
| 1 | Physical | Signal transmission |

---

# Memory Trick

```text
All

People

Seem

To

Need

Data

Processing
```

Or

```text
Please

Do

Not

Throw

Sausage

Pizza

Away
```

Use whichever you prefer.

---

# Layer 7: Application Layer

## Purpose

Interacts with users and applications.

---

## Responsibilities

Provides network services.

Examples:

```text
Web Browsers

Email

SSH

FTP

DNS Clients
```

---

## Protocol Examples

```text
HTTP

HTTPS

FTP

SMTP

SSH

DNS
```

---

## Example

You type:

```text
google.com
```

Browser begins communication.

---

# Layer 6: Presentation Layer

## Purpose

Makes data understandable.

Responsibilities:

```text
Formatting

Encryption

Compression

Encoding
```

---

## Examples

```text
TLS

SSL

UTF-8

JPEG

PNG
```

---

## Example

```text
Raw Data

↓

Encrypted Data

↓

Compressed Data
```

---

# Layer 5: Session Layer

## Purpose

Creates and manages conversations.

Responsibilities:

```text
Session creation

Session maintenance

Session termination
```

---

## Example

```text
Login Session

↓

Authenticated Session

↓

Logout
```

---

# Layer 4: Transport Layer

## Purpose

Reliable end-to-end communication.

Responsibilities:

```text
Segmentation

Error detection

Flow control

Reliable delivery
```

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
Acknowledgements

Ordering

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
No acknowledgements

No ordering

No retransmission
```

Examples:

```text
Gaming

Streaming

Video Calls

DNS
```

---

# Layer 3: Network Layer

## Purpose

Moves packets between networks.

Responsibilities:

```text
Addressing

Routing

Path selection
```

---

## Protocol Examples

```text
IP

ICMP
```

---

# Devices

```text
Routers
```

---

# Example

```text
Laptop

↓

Home Router

↓

ISP

↓

Google
```

---

# Layer 2: Data Link Layer

## Purpose

Moves data inside local networks.

Responsibilities:

```text
MAC addressing

Frame creation

Error detection
```

---

# Devices

```text
Switches
```

---

# Examples

```text
Ethernet

WiFi
```

---

# Layer 1: Physical Layer

## Purpose

Moves bits physically.

Examples:

```text
Electrical Signals

Radio Waves

Fiber Optics
```

---

# Devices

```text
Cables

NICs

Wireless Antennas

Fiber Links
```

---

# End To End Example

Suppose you visit:

```text
https://google.com
```

The journey looks like:

```text
Browser

↓

Application Layer

↓

Presentation Layer

↓

Session Layer

↓

Transport Layer

↓

Network Layer

↓

Data Link Layer

↓

Physical Layer

↓

Internet

↓

Google Server
```

---

# Encapsulation

Every layer adds information.

Visualization:

```text
Application Data

↓

[Application Header]

↓

[Presentation Header]

↓

[Session Header]

↓

[TCP Header]

↓

[IP Header]

↓

[Ethernet Header]

↓

Bits
```

---

# Decapsulation

Destination reverses everything.

```text
Bits

↓

Frame

↓

Packet

↓

Segment

↓

Session

↓

Presentation

↓

Application
```

---

# Data Names At Each Layer

| Layer | Data Unit |
|------|-----------|
| Application | Data |
| Presentation | Data |
| Session | Data |
| Transport | Segment |
| Network | Packet |
| Data Link | Frame |
| Physical | Bits |

---

# OSI Visual Diagram

```text
┌─────────────────────┐
│ Application         │
├─────────────────────┤
│ Presentation        │
├─────────────────────┤
│ Session             │
├─────────────────────┤
│ Transport           │
├─────────────────────┤
│ Network             │
├─────────────────────┤
│ Data Link           │
├─────────────────────┤
│ Physical            │
└─────────────────────┘
```

---

# Detailed Request Flow

```text
User

↓

Browser

↓

DNS Lookup

↓

TCP Handshake

↓

TLS Encryption

↓

HTTP Request

↓

Routing

↓

Ethernet Frame

↓

Signals

↓

Internet

↓

Google Server
```

---

# Linux Perspective

Linux does not strictly implement OSI.

Linux mainly uses:

```text
TCP/IP Stack
```

But Linux engineers still use OSI for troubleshooting.

---

# Linux Mapping

| OSI Layer | Linux Examples |
|-----------|---------------|
| Application | Browser, curl, nginx |
| Presentation | TLS, SSL |
| Session | SSH Sessions |
| Transport | TCP, UDP |
| Network | IP, Routing |
| Data Link | Ethernet, ARP |
| Physical | NIC, Cable |

---

# Linux Networking Internals

Packet journey:

```text
Application

↓

Socket API

↓

Kernel TCP/IP Stack

↓

Routing Table

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

# Modern Infrastructure Mapping

## Docker

Uses:

```text
Namespaces

Virtual Ethernet

Bridges
```

---

## Kubernetes

Uses:

```text
Pods

Services

Ingress

DNS
```

---

## Cloud

Uses:

```text
Virtual Networks

Subnets

Gateways

Load Balancers
```

---

# Production Example

User requests:

```text
amazon.com
```

Flow:

```text
Browser

↓

DNS

↓

Load Balancer

↓

Web Server

↓

API

↓

Database
```

Every layer participates.

---

# Troubleshooting Using OSI

This is one of the biggest reasons engineers learn OSI.

---

## Layer 1 Problems

```text
Cable unplugged

WiFi disabled

NIC failure
```

Tools:

```bash
ip link
ethtool
```

---

## Layer 2 Problems

```text
ARP issues

Switch issues

VLAN issues
```

Tools:

```bash
arp
ip neigh
```

---

## Layer 3 Problems

```text
Routing issues

IP conflicts

Gateway failures
```

Tools:

```bash
ip route
ping
traceroute
```

---

## Layer 4 Problems

```text
Port blocked

TCP failures

UDP failures
```

Tools:

```bash
ss
netstat
```

---

## Layer 5 Problems

```text
Session timeout

Authentication issues
```

---

## Layer 6 Problems

```text
TLS failures

Certificate issues
```

Tools:

```bash
openssl
```

---

## Layer 7 Problems

```text
Application bugs

API failures

Server crashes
```

Tools:

```bash
curl
```

---

# Real World Engineer Thinking

When something fails:

Do NOT panic.

Move layer by layer.

```text
Layer 1

↓

Layer 2

↓

Layer 3

↓

Layer 4

↓

Layer 5

↓

Layer 6

↓

Layer 7
```

This is how engineers debug production systems.

---

# WH Questions

## What is OSI?

A networking framework.

---

## Why was OSI created?

To organize networking responsibilities.

---

## Why do layers exist?

To separate responsibilities.

---

## Why is OSI still taught?

It simplifies learning and troubleshooting.

---

## Why does Linux use it?

Linux uses it as a mental model.

---

## Why is troubleshooting easier with OSI?

Because problems can be isolated layer by layer.

---

# Key Takeaways

Always think:

```text
Application

↓

Formatting

↓

Sessions

↓

Reliable Communication

↓

Routing

↓

Local Communication

↓

Signals
```

---

# What's Next?

```text
tcp-ip-model.md
```

You will learn:

- The actual model Linux uses
- How Linux networking really works
- How TCP/IP differs from OSI
