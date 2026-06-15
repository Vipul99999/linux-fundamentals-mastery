# UDP (User Datagram Protocol)

> Learn why modern systems intentionally choose an unreliable protocol, how Linux processes UDP traffic, and why speed sometimes matters more than reliability.

---

# Why Learn UDP?

Imagine you're on a video call.

Suddenly one video frame is lost.

Question:

```text
Should we stop everything

↓

Recover the missing frame

↓

Then continue?
```

No.

That would create delays.

Instead:

```text
Ignore it

↓

Continue
```

This is UDP's philosophy.

---

# The Big Question

Question:

```text
Why does an unreliable protocol power so much of the internet?
```

Examples:

```text
DNS

Video Calls

Gaming

Streaming

IoT

QUIC

Service Discovery
```

Because sometimes:

```text
Speed

>

Perfect Reliability
```

---

# Simple Definition

UDP is:

> A connectionless transport protocol that prioritizes speed and simplicity over reliability.

---

# Mental Model ⭐⭐⭐⭐⭐

Think:

```text
Send

↓

Forget

↓

Continue
```

UDP does not babysit packets.

---

# Big Picture

```text
Application

↓

UDP

↓

IP

↓

Ethernet

↓

Destination
```

---

# Why UDP Exists

Imagine if every packet needed:

```text
Connection Setup

↓

Tracking

↓

Acknowledgements

↓

Recovery

↓

Ordering
```

Everything would become slower.

UDP removes this overhead.

---

# UDP Philosophy ⭐⭐⭐⭐⭐

TCP philosophy:

```text
Make sure everything arrives.
```

UDP philosophy:

```text
Send immediately.
```

---

# House Delivery Analogy

TCP:

```text
Certified Delivery

↓

Track Everything

↓

Signatures

↓

Resend Missing Pages
```

UDP:

```text
Throw Flyer In Mailbox

↓

Move On
```

---

# What UDP Does NOT Provide ⭐⭐⭐⭐⭐

UDP does NOT provide:

```text
Connection Setup

↓

Reliability

↓

Ordering

↓

Retransmission

↓

Flow Control

↓

Congestion Control
```

---

# What UDP Does Provide

UDP provides:

```text
Very Low Overhead

↓

Fast Delivery

↓

Simple Communication
```

---

# UDP Packet Lifecycle ⭐⭐⭐⭐⭐

```text
Application

↓

Create Datagram

↓

UDP

↓

IP

↓

Network

↓

Destination
```

Done.

No connection setup.

---

# Connectionless Communication ⭐⭐⭐⭐⭐

This is extremely important.

TCP:

```text
Connect First

↓

Exchange Data
```

UDP:

```text
Send Immediately
```

---

# Visualization

TCP:

```text
Handshake

↓

Data
```

UDP:

```text
Data
```

---

# No Three-Way Handshake ⭐⭐⭐⭐⭐

TCP:

```text
SYN

↓

SYN ACK

↓

ACK
```

UDP:

```text
Nothing
```

Send immediately.

---

# Datagram Concept ⭐⭐⭐⭐⭐

UDP packets are called:

```text
Datagrams
```

Think:

```text
Independent Messages
```

Every datagram is separate.

---

# Visualization

```text
Datagram 1

↓

Datagram 2

↓

Datagram 3
```

No relationship.

---

# What Happens If A Packet Is Lost?

Example:

```text
1

2

4

5
```

UDP says:

```text
Continue
```

No recovery.

---

# What Happens If Packets Arrive Out Of Order?

Example:

```text
4

1

5

2
```

UDP says:

```text
Not My Problem
```

Applications handle it.

---

# Why Is UDP Fast? ⭐⭐⭐⭐⭐

Because it avoids:

```text
Handshake

ACKs

State Tracking

Recovery

Timers

Complex Algorithms
```

---

# Ports ⭐⭐⭐⭐⭐

UDP also uses ports.

Examples:

```text
53

DNS


67

DHCP Server


68

DHCP Client


123

NTP
```

---

# Visualization

```text
IP

↓

Port

↓

Application
```

---

# Common UDP Applications ⭐⭐⭐⭐⭐

---

# DNS

```text
Domain

↓

IP
```

Fast lookups.

---

# DHCP

```text
Assign Network Identity
```

Fast communication.

---

# Video Calls

Examples:

```text
Zoom

Google Meet

Teams
```

Low latency matters.

---

# Gaming

Examples:

```text
Multiplayer Games

Real-Time Events
```

Low latency matters.

---

# Streaming

Examples:

```text
Live Video

Voice Chat
```

---

# IoT

Examples:

```text
Sensors

Devices

Smart Homes
```

---

# QUIC ⭐⭐⭐⭐⭐

Modern HTTP/3 uses:

```text
UDP
```

Very important.

---

# Why Is QUIC Built On UDP?

Because developers wanted:

```text
TCP Reliability

+

UDP Flexibility
```

QUIC builds reliability in user space.

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux treats UDP differently.

Linux does NOT track connection state like TCP.

---

# Visualization

TCP:

```text
State Tracking
```

UDP:

```text
Minimal Tracking
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```bash
dig google.com
```

Linux performs:

```text
Application

↓

Socket

↓

UDP

↓

IP

↓

NIC

↓

Internet
```

Very lightweight.

---

# Linux Internal Architecture ⭐⭐⭐⭐⭐

```text
User Space

Application

↓

==================

Kernel Space

Socket

↓

UDP Stack

↓

IP

↓

NIC Driver

↓

NIC
```

---

# UDP Header ⭐⭐⭐⭐⭐

Very small.

Contains:

```text
Source Port

Destination Port

Length

Checksum
```

---

# Visualization

```text
┌────────────────────┐
│ Source Port        │
├────────────────────┤
│ Destination Port   │
├────────────────────┤
│ Length             │
├────────────────────┤
│ Checksum           │
└────────────────────┘
```

Only 8 bytes.

Very efficient.

---

# Socket Concept ⭐⭐⭐⭐⭐

Like TCP, UDP uses:

```text
Source IP

+

Source Port

+

Destination IP

+

Destination Port
```

---

# Linux Commands

Show UDP sockets:

```bash
ss -u
```

---

Show listening sockets:

```bash
ss -uln
```

---

Statistics:

```bash
ss -s
```

---

Capture packets:

```bash
tcpdump udp
```

---

# Real Production Example ⭐⭐⭐⭐⭐

Suppose:

```text
100,000 players

↓

Online Game
```

Using TCP:

```text
High Latency
```

Using UDP:

```text
Real-Time Experience
```

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud systems use UDP heavily.

Examples:

```text
DNS

Service Discovery

Streaming

QUIC
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

Containers support UDP.

Example:

```text
Container A

↓

UDP

↓

Container B
```

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Services may expose:

```text
TCP

UDP
```

Examples:

```text
DNS Services

Gaming Servers

Streaming Systems
```

---

# Security Perspective ⭐⭐⭐⭐⭐

Common risks:

```text
UDP Floods

Amplification Attacks

Reflection Attacks
```

---

# Amplification Attack Concept

Attacker abuses services.

Small request.

Huge response.

Servers become overloaded.

---

# Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
DNS failing
```

Check:

```bash
ss -uln
```

---

Problem:

```text
Gaming lag
```

Check:

```text
Packet loss

Latency

Jitter
```

---

Problem:

```text
Video freezing
```

Check:

```text
Network quality
```

---

# TCP vs UDP ⭐⭐⭐⭐⭐

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Yes | No |
| Reliable | Yes | No |
| Ordered | Yes | No |
| Retransmission | Yes | No |
| Flow Control | Yes | No |
| Congestion Control | Yes | No |
| Speed | Slower | Faster |

---

# Linux Internal Comparison ⭐⭐⭐⭐⭐

TCP:

```text
Application

↓

Socket

↓

TCP

↓

Track

↓

Recover

↓

Deliver
```

UDP:

```text
Application

↓

Socket

↓

UDP

↓

Send

↓

Deliver
```

---

# Packet Journey ⭐⭐⭐⭐⭐

Suppose:

```bash
dig google.com
```

Linux performs:

```text
Application

↓

UDP

↓

IP

↓

Router

↓

DNS Server

↓

Reply

↓

Application
```

---

# Common Misconceptions

❌ UDP is bad.

Wrong.

UDP is optimized for different goals.

---

❌ UDP is always faster.

Wrong.

Depends on workload.

---

❌ UDP means no error detection.

Wrong.

Checksums exist.

---

❌ UDP is obsolete.

Wrong.

HTTP/3 uses UDP.

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
Reliable

↓

Good

Unreliable

↓

Bad
```

Think:

```text
Choose The Right Tool
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Application

↓

UDP

↓

Fast

↓

Simple

↓

Low Overhead

↓

Destination
```

---

# WH Questions

## What is UDP?

Fast transport protocol.

---

## Why does UDP exist?

Speed and simplicity.

---

## Why use UDP?

Latency-sensitive systems.

---

## Why don't games use TCP?

Latency.

---

## Why does QUIC use UDP?

Flexibility.

---

## Why do engineers care?

Modern systems depend on UDP.

---

# Key Takeaways

Remember forever.

```text
Send

↓

Forget

↓

Continue
```

---

# Recommended Reading Order

```text
tcp.md

↓

udp.md

↓

tcp-vs-udp.md ⭐⭐⭐⭐⭐

↓

sockets.md
```
