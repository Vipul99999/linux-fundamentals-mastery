# Unicast vs Multicast vs Broadcast vs Anycast

> Learn the four communication patterns used in networking and how Linux, cloud systems, and modern infrastructure decide who should receive data.

---

# Why Learn This?

One of the first questions every network must answer is:

> Who should receive this data?

Sometimes:

```text
One device
```

Sometimes:

```text
Many devices
```

Sometimes:

```text
Everybody
```

Sometimes:

```text
The nearest available service
```

These communication patterns solve those problems.

Without them:

- DNS wouldn't scale
- CDNs wouldn't work
- Streaming would be inefficient
- Kubernetes networking would become expensive
- Cloud infrastructure would be harder to build

---

# The Four Communication Types

```text
1. Unicast

2. Broadcast

3. Multicast

4. Anycast
```

---

# Big Picture

```text
Sender

↓

Who should receive this?

↓

One Device

↓

Many Selected Devices

↓

Everyone

↓

Nearest Device
```

---

# Simple Analogy

Imagine a teacher in a classroom.

---

# Unicast

Teacher talks to one student.

```text
Teacher

↓

Student A
```

---

# Broadcast

Teacher talks to everybody.

```text
Teacher

↙ ↓ ↓ ↓ ↘

Everyone
```

---

# Multicast

Teacher talks to selected students.

```text
Teacher

↙     ↘

Group Members
```

---

# Anycast

Teacher talks to the nearest available teacher assistant.

```text
Teacher

↓

Nearest Assistant
```

---

# Comparison Table

| Type | Meaning | Recipients |
|------|---------|------------|
| Unicast | One to One | Single destination |
| Broadcast | One to All | Entire network |
| Multicast | One to Many | Selected group |
| Anycast | One to Nearest | Closest service |

---

# 1. Unicast

## Definition

One sender communicates with one receiver.

---

# Visualization

```text
Laptop

↓

Google Server
```

---

# Examples

```text
HTTPS

SSH

FTP

Database connections

API calls
```

---

# Real World Examples

Opening:

```text
https://google.com
```

Communication:

```text
Your Laptop

↓

Google Server
```

---

# Linux Example

```bash
curl https://google.com

ssh user@server
```

Both use unicast.

---

# Internals

Packet contains:

```text
Source IP

↓

Destination IP
```

Router forwards packet to that single destination.

---

# Unicast Visualization

```text
Client

↓

Router

↓

Server
```

---

# Advantages

```text
Simple

Reliable

Secure

Easy to manage
```

---

# Disadvantages

Large-scale duplication is expensive.

Example:

```text
1000 users

↓

1000 individual streams
```

---

# 2. Broadcast

## Definition

One sender communicates with every device inside a local network.

---

# Visualization

```text
Computer

↙ ↓ ↓ ↓ ↘

Everyone
```

---

# Why Does Broadcast Exist?

Sometimes a device doesn't know who it needs.

Example:

```text
Who owns IP 192.168.1.20?
```

Broadcast asks everyone.

---

# Real Example

ARP.

```text
Who owns this IP?
```

Everyone receives.

Only one answers.

---

# Linux Example

```text
ARP Request
```

Broadcast address:

```text
255.255.255.255
```

or

```text
192.168.1.255
```

depending on subnet.

---

# Broadcast Visualization

```text
Laptop

↙ ↓ ↓ ↓ ↘

Device A

Device B

Device C

Device D
```

---

# Advantages

```text
Simple discovery

Automatic detection
```

---

# Problems

Too much broadcast traffic causes:

```text
Broadcast Storm
```

---

# Broadcast Storm Visualization

```text
Device

↓

Everyone

↓

Everyone responds

↓

Congestion
```

---

# Modern Networks Avoid Excessive Broadcast

Because:

```text
Performance decreases
```

---

# 3. Multicast

## Definition

One sender communicates with selected receivers.

---

# Visualization

```text
Sender

↙     ↘

Receiver A

Receiver B
```

---

# Why Does Multicast Exist?

Efficiency.

Instead of:

```text
1000 copies
```

Send:

```text
1 stream

↓

Interested users receive it
```

---

# Examples

```text
IPTV

Stock exchanges

Video conferencing

Live streaming

Monitoring systems
```

---

# Multicast Group Example

```text
239.1.1.1
```

Devices join the group.

---

# Internal Working

Devices say:

```text
I want this stream
```

Router maintains membership.

---

# Visualization

```text
Video Server

↓

Multicast Group

↙ ↓ ↘

User A

User B

User C
```

---

# Advantages

```text
Efficient

Scalable

Less bandwidth usage
```

---

# Disadvantages

More complex configuration.

---

# 4. Anycast

## Definition

One sender communicates with the nearest available destination.

---

# Why Does Anycast Exist?

Speed.

Example:

```text
Google DNS
```

There are many servers worldwide.

You connect to the nearest one.

---

# Visualization

```text
User

↓

Nearest Server
```

---

# Real Examples

```text
CDNs

Google DNS

Cloudflare DNS

Global Load Balancers
```

---

# Anycast Visualization

```text
User India

↓

India Server


User Europe

↓

Europe Server


User USA

↓

USA Server
```

Same IP.

Different physical servers.

---

# Advantages

```text
Fast

Reliable

Scalable
```

---

# Internal Working

Routers choose:

```text
Shortest route
```

using routing protocols.

---

# Side By Side Visual

```text
UNICAST

A → B


BROADCAST

A → Everyone


MULTICAST

A → Group


ANYCAST

A → Nearest
```

---

# Linux Perspective

Linux mostly deals with:

```text
Unicast

Broadcast

Multicast
```

Anycast is usually configured in larger infrastructures.

---

# Linux Internals

Linux kernel networking stack supports all four.

Internally:

```text
Application

↓

Socket

↓

TCP/UDP

↓

IP

↓

Routing

↓

NIC
```

Packet forwarding decisions determine communication type.

---

# Linux Commands

---

## View multicast addresses

```bash
ip maddr
```

---

## View interfaces

```bash
ip addr
```

---

## View routing

```bash
ip route
```

---

# Real World Usage

---

# Home Network

Uses:

```text
Unicast

Broadcast
```

---

# Office Network

Uses:

```text
Unicast

Broadcast

Multicast
```

---

# Cloud Infrastructure

Uses:

```text
Unicast

Anycast
```

---

# Kubernetes

Mostly uses:

```text
Unicast
```

---

# Docker

Mostly uses:

```text
Unicast
```

---

# Internet Services

Uses:

```text
Unicast

Anycast
```

---

# Production Example

Opening YouTube.

```text
DNS

↓

Anycast

↓

CDN

↓

Unicast

↓

Your Device
```

---

# Troubleshooting

---

# Unicast Problems

```text
Routing issue

Firewall issue

Server unavailable
```

Tools:

```bash
ping

traceroute

curl
```

---

# Broadcast Problems

```text
Broadcast storm

ARP issue
```

Tools:

```bash
arp

ip neigh
```

---

# Multicast Problems

```text
Group membership issue

Router configuration issue
```

Tools:

```bash
ip maddr
```

---

# Anycast Problems

```text
Wrong nearest server

Routing issue
```

Tools:

```bash
traceroute
```

---

# Security Considerations

---

# Broadcast

Can be abused.

Examples:

```text
ARP spoofing

Broadcast storms
```

---

# Multicast

Unauthorized users may join groups.

---

# Anycast

Requires careful routing configuration.

---

# Engineer Mental Model

Whenever data is sent, ask:

```text
Who should receive this?
```

Then answer:

```text
One?

↓

Unicast


Everyone?

↓

Broadcast


Specific group?

↓

Multicast


Nearest available?

↓

Anycast
```

---

# WH Questions

## What is unicast?

One-to-one communication.

---

## What is broadcast?

One-to-all communication.

---

## What is multicast?

One-to-many selected communication.

---

## What is anycast?

One-to-nearest communication.

---

## Why does anycast exist?

To improve speed and scalability.

---

## Why is broadcast limited?

Too much broadcast hurts performance.

---

## Why is multicast efficient?

One stream serves many users.

---

# Key Takeaways

Remember this forever.

```text
UNICAST

1 → 1


BROADCAST

1 → ALL


MULTICAST

1 → MANY


ANYCAST

1 → NEAREST
```

---

# What's Next?

```text
mtu.md
```

This file is much more important than people realize because it explains:

> Why packets cannot be infinitely large and why fragmentation causes performance issues.
