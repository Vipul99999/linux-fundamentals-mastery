# TCP (Transmission Control Protocol)

> Learn how reliable communication works on the internet, how Linux manages millions of connections, and why modern infrastructure depends on TCP.

---

# Why Learn TCP?

Imagine the internet without reliability.

You click:

```text
google.com
```

Possibilities:

```text
Half the page loads

↓

Images disappear

↓

Videos fail

↓

Packets arrive randomly

↓

Data gets corrupted
```

Chaos.

TCP solves this.

---

# The Big Question

Suppose:

```text
You open github.com
```

Question:

```text
How do billions of packets arrive correctly?

How do they arrive in order?

How are missing packets recovered?
```

Answer:

```text
TCP
```

---

# Simple Definition

TCP is:

> A connection-oriented protocol that provides reliable, ordered, and error-checked communication.

---

# Mental Model ⭐⭐⭐⭐⭐

Think:

```text
Application Data

↓

Break Into Pieces

↓

Track Everything

↓

Recover Failures

↓

Reassemble Correctly
```

TCP is a manager.

---

# Why TCP Exists

IP only says:

```text
Deliver this packet
```

IP does NOT guarantee:

```text
Delivery

Order

Reliability
```

TCP adds those guarantees.

---

# Big Picture

```text
Application

↓

TCP

↓

IP

↓

Ethernet

↓

Destination
```

---

# What TCP Provides ⭐⭐⭐⭐⭐

TCP gives:

```text
Reliable Delivery

↓

Ordered Delivery

↓

Error Detection

↓

Retransmission

↓

Flow Control

↓

Congestion Control

↓

Connection Management
```

---

# House Delivery Analogy

Imagine shipping a 1000-page book.

Without TCP:

```text
Page 500

↓

Page 10

↓

Page 700

↓

Page 100
```

Chaos.

TCP does:

```text
Page Numbers

↓

Missing Page Detection

↓

Resend Missing Pages

↓

Reassemble Book
```

---

# TCP Lifecycle ⭐⭐⭐⭐⭐

```text
Create Connection

↓

Exchange Data

↓

Close Connection
```

Three phases.

---

# Phase 1: Connection Setup

TCP first creates trust.

This is called:

```text
3-Way Handshake
```

---

# Three-Way Handshake ⭐⭐⭐⭐⭐

Step 1:

```text
SYN
```

Client says:

```text
Can we communicate?
```

---

Step 2:

```text
SYN + ACK
```

Server says:

```text
Yes.
I am ready.
```

---

Step 3:

```text
ACK
```

Client says:

```text
Great.
Let's begin.
```

---

# Visualization ⭐⭐⭐⭐⭐

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

Connection Established
```

---

# Real Example

Suppose:

```text
Browser

↓

github.com
```

Linux performs:

```text
DNS

↓

TCP Handshake

↓

TLS

↓

HTTP
```

TCP happens before HTTP.

---

# Phase 2: Data Transfer

Data starts flowing.

Visualization:

```text
Application Data

↓

Segment

↓

Segment

↓

Segment

↓

Destination
```

---

# Sequence Numbers ⭐⭐⭐⭐⭐

Every segment gets a number.

Example:

```text
1

2

3

4

5
```

This allows ordering.

---

# Visualization

```text
Packet 1

↓

Packet 2

↓

Packet 3

↓

Packet 4
```

Destination reconstructs.

---

# What If Packet 3 Is Lost?

Example:

```text
1

2

4

5
```

TCP detects:

```text
Packet 3 Missing
```

and retransmits it.

---

# Retransmission ⭐⭐⭐⭐⭐

Visualization:

```text
Packet Lost

↓

Detect Loss

↓

Resend

↓

Continue
```

---

# Acknowledgements (ACK) ⭐⭐⭐⭐⭐

Destination confirms reception.

Example:

```text
Received 1

Received 2

Received 3
```

ACKs continuously flow.

---

# Visualization

```text
Data

↓

ACK

↓

Data

↓

ACK
```

---

# Sliding Window ⭐⭐⭐⭐⭐

TCP doesn't send one packet at a time.

It sends batches.

Visualization:

```text
1

2

3

4

5

↓

ACK
```

This improves speed.

---

# Flow Control ⭐⭐⭐⭐⭐

Question:

```text
Can receiver keep up?
```

TCP prevents overwhelming slower systems.

Visualization:

```text
Sender

↓

Too Fast?

↓

Slow Down
```

---

# Congestion Control ⭐⭐⭐⭐⭐

Question:

```text
Can the network handle this traffic?
```

TCP adjusts automatically.

Visualization:

```text
Fast

↓

Congestion

↓

Slow Down

↓

Recover
```

---

# TCP Congestion Algorithms ⭐⭐⭐⭐⭐

Examples:

```text
Reno

CUBIC

BBR
```

Linux commonly uses:

```text
CUBIC
```

Modern systems increasingly use:

```text
BBR
```

---

# TCP Connection State Machine ⭐⭐⭐⭐⭐

Linux tracks every connection.

Visualization:

```text
LISTEN

↓

SYN_SENT

↓

SYN_RECEIVED

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

# ESTABLISHED

Connection active.

---

# FIN_WAIT

Closing process started.

---

# TIME_WAIT

Linux waits before fully closing.

This prevents delayed packet problems.

---

# Why TIME_WAIT Exists ⭐⭐⭐⭐⭐

Without it:

```text
Old packets

↓

Corrupt new connections
```

Linux waits for safety.

---

# Ports ⭐⭐⭐⭐⭐

TCP uses ports.

Examples:

```text
80

HTTP


443

HTTPS


22

SSH


5432

PostgreSQL
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

# Socket Concept ⭐⭐⭐⭐⭐

A TCP connection is identified by:

```text
Source IP

+

Source Port

+

Destination IP

+

Destination Port
```

This is called:

```text
4-Tuple
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux stores every TCP connection.

View them:

```bash
ss -t
```

---

# Show Listening Services

```bash
ss -tuln
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```bash
curl github.com
```

Linux performs:

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

NIC

↓

Internet
```

---

# Linux Internal Architecture ⭐⭐⭐⭐⭐

```text
User Space

Application

↓

=================

Kernel Space

Socket

↓

TCP Stack

↓

IP

↓

NIC Driver

↓

NIC
```

---

# TCP Buffers ⭐⭐⭐⭐⭐

Linux allocates memory.

Visualization:

```text
Application

↓

Send Buffer

↓

TCP

↓

Receive Buffer

↓

Application
```

---

# Why Does TCP Consume RAM?

Millions of connections require:

```text
State Tracking

Buffers

Timers

Sequence Numbers
```

---

# Real Production Example ⭐⭐⭐⭐⭐

Suppose:

```text
100,000 users

↓

Open website
```

Server manages:

```text
100,000 TCP connections
```

simultaneously.

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud systems heavily depend on TCP.

Examples:

```text
APIs

Databases

Microservices

Load Balancers
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

Containers use TCP constantly.

Visualization:

```text
Container A

↓

TCP

↓

Container B
```

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Every service uses TCP heavily.

Examples:

```text
API Server

etcd

Services

Pods
```

---

# Security Perspective ⭐⭐⭐⭐⭐

Common attacks:

```text
SYN Flood

RST Attack

Connection Exhaustion
```

---

# SYN Flood

Attacker creates many fake handshakes.

Visualization:

```text
SYN

SYN

SYN

SYN

↓

Resources Exhausted
```

---

# Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
Website loads slowly
```

Check:

```text
Packet loss?

Congestion?

Retransmissions?
```

---

Problem:

```text
Connection refused
```

Check:

```bash
ss -tuln
```

---

Problem:

```text
Too many connections
```

Check:

```bash
ss -s
```

---

# Linux Commands

Show TCP connections:

```bash
ss -t
```

---

Show all listening:

```bash
ss -tuln
```

---

Statistics:

```bash
ss -s
```

---

Packet capture:

```bash
tcpdump tcp
```

---

# Packet Journey ⭐⭐⭐⭐⭐

Suppose:

```text
https://github.com
```

Linux performs:

```text
DNS

↓

TCP Handshake

↓

TLS

↓

HTTP

↓

Response

↓

Close Connection
```

---

# Common Misconceptions

❌ TCP guarantees speed.

Wrong.

It guarantees reliability.

---

❌ TCP = Internet.

Wrong.

TCP is one protocol.

---

❌ TCP never loses packets.

Wrong.

It recovers from packet loss.

---

❌ TCP is stateless.

Wrong.

TCP is heavily stateful.

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
Data

↓

Destination
```

Think:

```text
Connection

↓

Track

↓

Recover

↓

Reorder

↓

Deliver
```

---

# TCP vs UDP Preview ⭐⭐⭐⭐⭐

TCP:

```text
Reliable

Ordered

Stateful
```

UDP:

```text
Fast

Simple

Stateless
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Application

↓

TCP

↓

Reliable Delivery

↓

Recover Failures

↓

Ordered Delivery

↓

Destination
```

---

# WH Questions

## What is TCP?

Reliable transport protocol.

---

## Why does TCP exist?

IP isn't reliable.

---

## Why does TCP need handshakes?

To establish communication.

---

## Why are sequence numbers needed?

To reorder packets.

---

## Why does Linux track states?

TCP is stateful.

---

## Why do engineers care?

Modern systems depend on TCP.

---

# Key Takeaways

Remember forever.

```text
Connection

↓

Track

↓

Recover

↓

Reorder

↓

Deliver
```

---

# Recommended Reading Order

```text
icmp.md

↓

tcp.md

↓

udp.md

↓

sockets.md
```
