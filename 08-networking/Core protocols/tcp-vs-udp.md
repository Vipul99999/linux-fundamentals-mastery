# TCP vs UDP

> Learn how engineers choose between TCP and UDP, understand their tradeoffs, and discover why modern systems use both.

---

# Why This File Exists

One of the most common beginner questions is:

```text
Which one is better?

TCP?

or

UDP?
```

Wrong question.

The correct question is:

```text
Which problem am I solving?
```

---

# The Big Idea ⭐⭐⭐⭐⭐

TCP and UDP are not competitors.

They are tools.

Think:

```text
Hammer

↓

Screwdriver
```

Neither is better.

Choose the correct tool.

---

# Simple Definitions

## TCP

Reliable communication.

Think:

```text
Accuracy First
```

---

## UDP

Fast communication.

Think:

```text
Speed First
```

---

# Ultimate Mental Model ⭐⭐⭐⭐⭐

TCP says:

```text
Make sure everything arrives.
```

UDP says:

```text
Send immediately.
```

---

# One Sentence Difference ⭐⭐⭐⭐⭐

TCP:

```text
Reliable Delivery
```

UDP:

```text
Fast Delivery
```

---

# Big Picture Architecture ⭐⭐⭐⭐⭐

```text
Application

↓

Transport Layer

↓

TCP or UDP

↓

IP

↓

Ethernet

↓

NIC

↓

Internet
```

---

# Philosophy Comparison ⭐⭐⭐⭐⭐

TCP:

```text
Safety

↓

Tracking

↓

Recovery

↓

Delivery
```

UDP:

```text
Speed

↓

Simplicity

↓

Delivery
```

---

# Human Analogy ⭐⭐⭐⭐⭐

TCP:

```text
Registered Courier Service

↓

Track Everything

↓

Signature Required

↓

Resend Missing Packages
```

UDP:

```text
Throw Flyer Into Mailbox

↓

Move On
```

---

# Feature Comparison ⭐⭐⭐⭐⭐

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection Oriented | ✅ | ❌ |
| Reliable | ✅ | ❌ |
| Ordered | ✅ | ❌ |
| Retransmission | ✅ | ❌ |
| Flow Control | ✅ | ❌ |
| Congestion Control | ✅ | ❌ |
| Handshake | ✅ | ❌ |
| Stateful | ✅ | ❌ |
| Latency | Higher | Lower |
| Overhead | Higher | Lower |

---

# The Biggest Difference ⭐⭐⭐⭐⭐

TCP creates:

```text
Relationship
```

UDP creates:

```text
Messages
```

---

# Connection Visual ⭐⭐⭐⭐⭐

TCP:

```text
Client

↓

Handshake

↓

Connection

↓

Data

↓

Close
```

UDP:

```text
Client

↓

Data

↓

Done
```

---

# Packet Journey Comparison ⭐⭐⭐⭐⭐

TCP:

```text
Application

↓

TCP

↓

Track

↓

Recover

↓

Deliver

↓

Destination
```

UDP:

```text
Application

↓

UDP

↓

Send

↓

Destination
```

---

# Three Way Handshake ⭐⭐⭐⭐⭐

TCP:

```text
Client

↓

SYN

↓

Server

↓

SYN ACK

↓

Client

↓

ACK

↓

Connection Ready
```

---

UDP:

```text
No Handshake
```

---

# What Happens If A Packet Is Lost?

TCP:

```text
Lost

↓

Detect

↓

Resend

↓

Continue
```

UDP:

```text
Lost

↓

Ignore

↓

Continue
```

---

# What Happens If Packets Arrive Out Of Order?

TCP:

```text
Packet 3

Packet 1

Packet 2

↓

Reorder

↓

Deliver
```

UDP:

```text
Packet 3

Packet 1

Packet 2

↓

Application Handles It
```

---

# Header Comparison ⭐⭐⭐⭐⭐

TCP Header:

```text
20-60 Bytes
```

Contains:

```text
Ports

Sequence Numbers

Acknowledgements

Flags

Window

Checksum
```

---

UDP Header:

```text
8 Bytes
```

Contains:

```text
Ports

Length

Checksum
```

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

Track State

↓

Buffers

↓

IP

↓

NIC
```

UDP:

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
```

---

# Memory Usage ⭐⭐⭐⭐⭐

TCP:

```text
High
```

Needs:

```text
Buffers

Timers

State Tracking

Recovery Data
```

---

UDP:

```text
Low
```

Needs:

```text
Minimal Information
```

---

# Linux State Machine ⭐⭐⭐⭐⭐

TCP:

```text
LISTEN

↓

SYN_SENT

↓

ESTABLISHED

↓

FIN_WAIT

↓

TIME_WAIT

↓

CLOSED
```

---

UDP:

```text
Send

↓

Done
```

---

# Linux Commands ⭐⭐⭐⭐⭐

Show TCP:

```bash
ss -t
```

---

Show UDP:

```bash
ss -u
```

---

Show both:

```bash
ss -tuln
```

---

# Real World Usage ⭐⭐⭐⭐⭐

## TCP Uses

```text
Websites

HTTPS

SSH

Databases

APIs

Git

Email
```

---

## UDP Uses

```text
DNS

Gaming

Video Calls

Streaming

IoT

QUIC

NTP
```

---

# Website Example ⭐⭐⭐⭐⭐

Opening:

```text
https://github.com
```

Uses:

```text
TCP
```

Because:

```text
Missing HTML

↓

Broken Website
```

Unacceptable.

---

# Video Call Example ⭐⭐⭐⭐⭐

Uses:

```text
UDP
```

Because:

```text
Missing One Frame

↓

Continue
```

Better experience.

---

# Gaming Example ⭐⭐⭐⭐⭐

Uses:

```text
UDP
```

Because:

```text
10ms Delay

↓

Player Feels Lag
```

---

# Database Example ⭐⭐⭐⭐⭐

Uses:

```text
TCP
```

Because:

```text
Lost Data

↓

Catastrophic
```

---

# DNS Example ⭐⭐⭐⭐⭐

Usually:

```text
UDP
```

Sometimes:

```text
TCP
```

For large responses.

---

# QUIC Example ⭐⭐⭐⭐⭐

HTTP/3 uses:

```text
UDP
```

Because developers wanted:

```text
TCP Reliability

+

UDP Flexibility
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

Containers use both.

```text
Container A

↓

TCP

↓

Container B


Container A

↓

UDP

↓

Container C
```

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Services support both.

```text
API Server

↓

TCP

DNS

↓

UDP
```

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud infrastructures use both heavily.

Examples:

```text
Load Balancers

Databases

Service Discovery

Streaming Systems
```

---

# Security Comparison ⭐⭐⭐⭐⭐

TCP Risks:

```text
SYN Flood

Connection Exhaustion

RST Attacks
```

---

UDP Risks:

```text
UDP Flood

Amplification

Reflection Attacks
```

---

# Troubleshooting Mindset ⭐⭐⭐⭐⭐

Slow Website:

Think:

```text
TCP
```

---

Gaming Lag:

Think:

```text
UDP
```

---

DNS Failure:

Think:

```text
UDP

Possibly TCP
```

---

# Infrastructure Visual ⭐⭐⭐⭐⭐

```text
Internet

↓

Load Balancer

↓

API Server

↓

Database


TCP


DNS


UDP


Streaming


UDP
```

---

# Engineer Decision Tree ⭐⭐⭐⭐⭐

Ask:

```text
Do I need every packet?

↓

YES

↓

TCP


NO

↓

Do I need very low latency?

↓

YES

↓

UDP
```

---

# Production Thinking ⭐⭐⭐⭐⭐

Engineers never ask:

```text
TCP

or

UDP?
```

They ask:

```text
Reliability Requirement?

↓

Latency Requirement?

↓

Traffic Type?

↓

User Experience Requirement?

↓

Choose Protocol
```

---

# Common Misconceptions

❌ UDP is bad.

Wrong.

---

❌ TCP is always better.

Wrong.

---

❌ Websites use UDP.

Mostly wrong (HTTP/1.1 and HTTP/2 use TCP).

HTTP/3 uses QUIC over UDP.

---

❌ Gaming should use TCP.

Wrong.

---

❌ Faster always means better.

Wrong.

---

# Memory Map ⭐⭐⭐⭐⭐

Remember forever.

```text
TCP

↓

Reliable

↓

Ordered

↓

Track

↓

Recover


UDP

↓

Fast

↓

Simple

↓

Send
```

---

# One Minute Revision ⭐⭐⭐⭐⭐

```text
TCP

↓

Handshake

↓

Track

↓

Recover

↓

Reliable


UDP

↓

Send

↓

Continue

↓

Fast


Choose Based On Requirements
```

---

# WH Questions

## Which one is better?

Neither.

Choose based on requirements.

---

## Why do websites use TCP?

Reliability.

---

## Why do games use UDP?

Low latency.

---

## Why is UDP fast?

Minimal overhead.

---

## Why is TCP slower?

State tracking.

---

## Why do engineers care?

Everything depends on choosing correctly.

---

# Ultimate Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
TCP

vs

UDP
```

Think:

```text
Requirements

↓

Tradeoffs

↓

Choose Tool
```

---

# Recommended Reading Order

```text
tcp.md

↓

udp.md

↓

tcp-vs-udp.md

↓

sockets.md
```
