# Networking Terminologies

> Learn the language of networking before learning networking itself.

This file teaches the fundamental terms used throughout Linux networking, cloud infrastructure, DevOps, containers, Kubernetes, distributed systems, and modern production environments.

The goal is not memorization.

The goal is to build intuition.

---

# Why Learn Networking Terminologies?

Networking is a language.

Imagine learning programming without understanding:

```text
Variable

Function

Class

Object
```

Networking is the same.

Without terminology knowledge:

```text
Packet analysis ❌

DNS debugging ❌

Firewall configuration ❌

Docker networking ❌

Kubernetes networking ❌
```

Understanding the language makes every future topic easier.

---

# Networking Big Picture

Everything in networking revolves around communication.

```text
Application

↓

Data

↓

Protocols

↓

Addresses

↓

Packets

↓

Routes

↓

Destination

↓

Response
```

---

# Core Networking Entities

```text
Device

↓

Interface

↓

Address

↓

Packet

↓

Protocol

↓

Router

↓

Destination
```

---

# 1. Network

## Definition

A network is a group of interconnected devices that exchange information.

---

## Real World Example

```text
Laptop

↓

Router

↓

Phone

↓

Printer
```

All connected.

This is a network.

---

## Internal Thinking

A network is simply:

```text
Communication infrastructure
```

---

# 2. Node

## Definition

Any device participating in a network.

Examples:

```text
Computer

Server

Phone

Router

Switch

Printer

Container

Virtual Machine
```

All are nodes.

---

## Internal Thinking

```text
Node

=

Participant
```

---

# 3. Host

## Definition

A device with an IP address that can send and receive data.

Examples:

```text
Laptop

Server

Virtual Machine

Container
```

---

## Internal Thinking

```text
Node

↓

Network Identity

↓

Host
```

Every host is a node.

Not every node is necessarily a host.

---

# 4. Client

## Definition

Requests a service.

Examples:

```text
Browser

↓

Website
```

Browser = Client

---

## Internal Thinking

```text
Initiates communication
```

---

# 5. Server

## Definition

Provides a service.

Examples:

```text
Web Server

Database Server

API Server

DNS Server
```

---

## Internal Thinking

```text
Waits for requests

↓

Processes requests

↓

Returns responses
```

---

# 6. Protocol

## Definition

A set of rules that devices agree upon.

---

## Human Example

Humans:

```text
Grammar

Vocabulary

Sentence rules
```

Networking:

```text
Protocol rules
```

---

## Examples

```text
HTTP

HTTPS

TCP

UDP

DNS

ARP

SSH
```

---

# Internal Thinking

Without protocols:

```text
Computer A

???

Computer B
```

Communication becomes impossible.

---

# 7. Data

## Definition

Information being transferred.

Examples:

```text
Images

Videos

Messages

Files

Commands
```

---

# Internal Thinking

```text
Raw Information
```

Protocols transport it.

---

# 8. Packet

## Definition

A small unit of transferred data.

---

## Example

Large file:

```text
500 MB
```

Becomes:

```text
Packet 1

Packet 2

Packet 3

...

Packet N
```

---

## Why Packets Exist?

Advantages:

```text
Efficient

Flexible

Fault tolerant
```

---

# Internal Structure

A packet usually contains:

```text
Header

↓

Payload
```

Example:

```text
Packet

├── Header
│   ├── Source Address
│   ├── Destination Address
│   └── Metadata

└── Payload
    └── Actual Data
```

---

# 9. Frame

## Definition

Data Link layer data unit.

---

## Internal Thinking

```text
Packet

↓

Wrapped

↓

Frame
```

A frame contains:

```text
MAC addresses

+

Packet
```

---

# 10. Segment

## Definition

Transport layer data unit.

Usually TCP.

---

## Visualization

```text
Application Data

↓

Segment

↓

Packet

↓

Frame

↓

Electrical Signals
```

---

# 11. Interface

## Definition

A communication point.

Examples:

```text
eth0

wlan0

lo

docker0
```

---

## Internal Thinking

Linux talks to networks through interfaces.

```text
Application

↓

Kernel

↓

Interface

↓

Network
```

---

# 12. Network Interface Card (NIC)

## Definition

Hardware responsible for network communication.

Examples:

```text
Ethernet Card

WiFi Card
```

---

# Internal Working

```text
CPU

↓

Kernel

↓

NIC

↓

Cable/WiFi
```

---

# 13. IP Address

## Definition

Logical network identity.

Example:

```text
192.168.1.100
```

---

## Purpose

Answers:

```text
Where should data go?
```

---

# 14. MAC Address

## Definition

Hardware identity.

Example:

```text
A4:5E:60:12:34:56
```

---

## Internal Thinking

```text
Physical Identity
```

---

# IP vs MAC

| Feature | IP | MAC |
|---------|----|-----|
| Type | Logical | Physical |
| Changes | Yes | Usually No |
| Scope | Network | Local Network |
| Used By | Routers | Switches |

---

# 15. Port

## Definition

A logical doorway for applications.

---

## Example

```text
IP Address

↓

Application Port

↓

Application
```

---

## Examples

```text
22 → SSH

53 → DNS

80 → HTTP

443 → HTTPS
```

---

# Internal Thinking

One machine can run many services.

Ports differentiate them.

---

# 16. Socket

## Definition

A communication endpoint.

---

## Internal Thinking

Socket = IP + Port + Protocol

Example:

```text
192.168.1.10

↓

443

↓

TCP
```

---

# 17. Router

## Definition

A device that forwards packets between networks.

---

## Internal Thinking

Router answers:

```text
Where should packet go?
```

---

# 18. Switch

## Definition

Connects devices inside a local network.

---

## Internal Thinking

Switch answers:

```text
Who inside this network should receive this?
```

---

# 19. Gateway

## Definition

Exit point from one network to another.

Example:

```text
Home Network

↓

Router

↓

Internet
```

Router often acts as gateway.

---

# 20. DNS

## Definition

Converts names into IP addresses.

---

Example:

```text
google.com

↓

142.x.x.x
```

---

# 21. ISP

## Definition

Internet Service Provider.

Examples:

```text
Jio

Airtel

ACT

BSNL
```

---

# Internal Thinking

ISP connects your network to the internet.

---

# 22. Latency

## Definition

Delay between sending and receiving.

Measured in:

```text
Milliseconds (ms)
```

---

# Example

```text
Request

↓

50 ms

↓

Response
```

---

# 23. Bandwidth

## Definition

Maximum amount of transferable data.

---

## Water Pipe Analogy

```text
Pipe width

=

Bandwidth
```

---

# 24. Throughput

## Definition

Actual amount of data transferred.

---

# Example

Bandwidth:

```text
100 Mbps
```

Actual:

```text
75 Mbps
```

Throughput:

```text
75 Mbps
```

---

# 25. MTU

## Definition

Maximum size of one packet.

---

# Internal Thinking

```text
Too large

↓

Fragmentation

↓

Performance issues
```

---

# Linux Networking Internals Overview

Linux networking roughly follows:

```text
Application

↓

Socket API

↓

Kernel

↓

TCP/IP Stack

↓

Routing Table

↓

Network Interface

↓

NIC

↓

Physical Network
```

---

# End-To-End Example

Suppose:

```text
https://google.com
```

Linux internally performs:

```text
Browser

↓

DNS Lookup

↓

IP Address

↓

TCP Handshake

↓

TLS Handshake

↓

HTTP Request

↓

Routing Decision

↓

Network Interface

↓

NIC

↓

Internet

↓

Google
```

---

# Production Systems Use These Everywhere

| System | Uses |
|--------|------|
| Docker | Interfaces, namespaces |
| Kubernetes | Routing, DNS |
| AWS | IP, routing |
| Nginx | Ports, sockets |
| Databases | TCP |
| APIs | TCP |
| VPNs | Tunnels |

---

# Troubleshooting Mindset

Always ask:

```text
Who is talking?

↓

Who is listening?

↓

How do they identify each other?

↓

Which protocol is used?

↓

What path is taken?

↓

Where did communication fail?
```

---

# WH Questions

## What is a host?

A network device with an IP address.

---

## What is a protocol?

Communication rules.

---

## Why do packets exist?

Efficient data transportation.

---

## Why do ports exist?

To separate applications.

---

## Why does DNS exist?

Humans cannot remember IP addresses.

---

## Why does routing exist?

Packets need paths.

---

## Why are interfaces important?

Linux communicates through them.

---

# Key Takeaways

Think in this order:

```text
Application

↓

Socket

↓

Protocol

↓

Packet

↓

Address

↓

Route

↓

Interface

↓

Network

↓

Destination
```

---

# Next File

```text
osi-model.md
```

You will learn the most important mental model in networking.
