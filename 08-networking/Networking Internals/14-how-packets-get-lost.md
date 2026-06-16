# Story 14: How Packets Get Lost ⭐⭐⭐⭐⭐

# Why This File Exists

This is one of the most important files in this entire repository.

Because production networking is not about moving packets.

It's about finding lost packets.

Most outages eventually become this question:

> Where did the packet die?

This mindset separates:

Junior Engineers

from

Senior Infrastructure Engineers.

This file teaches how packets fail.

Because packets die all the time.

And that's normal.

---

# Learning Goals

After reading this file, you should be able to answer:

- Why do packets get lost?
- Where do packets get lost?
- How do engineers find packet failures?
- Why does latency happen?
- Why do timeouts happen?
- Why do systems retry?
- Why do distributed systems fail?
- How do SREs think?

---

# The Biggest Misconception

People think:

```text
Browser

↓

Google
```

Wrong.

Reality:

```text
Browser

↓

DNS

↓

ARP

↓

Gateway

↓

Switch

↓

Router

↓

ISP

↓

Google Edge

↓

Load Balancer

↓

Google Server
```

Packets can die anywhere.

---

# Meet Penny Again

Penny has one mission.

```text
Reach destination.
```

Penny is born.

Journey begins.

Question:

Will Penny survive?

Not guaranteed.

---

# The Golden Rule ⭐⭐⭐⭐⭐

Every hop is a failure opportunity.

Memorize this.

Visualization:

```text
Laptop

↓

Gateway

↓

ISP

↓

Transit

↓

Cloud

↓

Data Center

↓

Server
```

Each arrow can fail.

---

# Failure Point #1 ⭐⭐⭐⭐⭐

# DNS Failure

You type:

```text
google.com
```

DNS fails.

Penny is never born.

Visualization:

```text
Human

↓

DNS

💥
```

No networking happens.

---

# Symptoms

```text
Cannot resolve hostname
```

Examples:

```bash
ping google.com
```

Fails.

But:

```bash
ping 8.8.8.8
```

Works.

Classic DNS issue.

---

# Failure Point #2 ⭐⭐⭐⭐⭐

# Application Failure

Application crashes.

Penny is never created.

Examples:

```text
Nginx down

API crashed

Database down
```

No packet.

No communication.

---

# Failure Point #3 ⭐⭐⭐⭐⭐

# Routing Failure

Linux asks:

```text
Where should packet go?
```

No route exists.

Packet dies.

Example:

```bash
ip route
```

Missing:

```text
default via
```

Internet unreachable.

---

# Failure Point #4 ⭐⭐⭐⭐⭐

# ARP Failure

Linux knows gateway IP.

But not MAC.

ARP fails.

Visualization:

```text
Laptop

↓

ARP

💥
```

No local communication.

---

# Symptoms

```text
Gateway unreachable.
```

Check:

```bash
ip neigh
```

---

# Failure Point #5 ⭐⭐⭐⭐⭐

# Switch Failure

Problems:

```text
Loop

Broadcast storm

Port failure

Misconfiguration
```

Packets disappear.

---

# Failure Point #6 ⭐⭐⭐⭐⭐

# Router Failure

Problems:

```text
Wrong routes

Missing routes

Congestion

Router crash
```

Packets disappear.

---

# Failure Point #7 ⭐⭐⭐⭐⭐

# Firewall Failure

Very common.

Firewall says:

```text
DROP
```

Packet dies.

---

# DROP vs REJECT ⭐⭐⭐⭐⭐

DROP:

```text
Ignore existence.
```

REJECT:

```text
Tell sender no.
```

Huge difference.

---

# Visualization

DROP:

```text
Packet

↓

💥

Silence
```

REJECT:

```text
Packet

↓

No
```

---

# Failure Point #8 ⭐⭐⭐⭐⭐

# NAT Failure

Translation breaks.

Examples:

```text
Port exhaustion

Wrong mappings
```

Very common in cloud systems.

---

# Failure Point #9 ⭐⭐⭐⭐⭐

# ISP Failure

Examples:

```text
Fiber cut

Congestion

Routing issue

Peering issue
```

Application may be healthy.

Network isn't.

---

# Failure Point #10 ⭐⭐⭐⭐⭐

# Load Balancer Failure

Examples:

```text
Bad health checks

Wrong targets

Configuration mistakes
```

Traffic disappears.

---

# Failure Point #11 ⭐⭐⭐⭐⭐

# Cloud Failure

Examples:

```text
Bad route table

Security group

NACL

IGW issue
```

Very common.

---

# Failure Point #12 ⭐⭐⭐⭐⭐

# Kubernetes Failure

Examples:

```text
Broken CNI

Broken kube-proxy

Broken Ingress

Broken DNS
```

Traffic disappears.

---

# Packet Loss Is Not Binary ⭐⭐⭐⭐⭐

Packets don't always completely fail.

Scenarios:

```text
0%

5%

10%

50%

100%
```

Packet loss.

This distinction matters.

---

# Why Retries Exist

Humans expect failures.

Applications retry.

Examples:

```text
Browser

Database

Microservices
```

Retries are everywhere.

---

# Why Timeouts Exist

Humans don't wait forever.

Examples:

```text
3 seconds

5 seconds

30 seconds
```

Eventually.

```text
Give up.
```

---

# Why Distributed Systems Are Hard

One request may activate:

```text
API

↓

Authentication

↓

Database

↓

Cache

↓

Queue

↓

Third Party API
```

Many opportunities for failure.

---

# Packet Death Zones ⭐⭐⭐⭐⭐

Packets usually die in one of these layers.

```text
Application

↓

DNS

↓

Linux

↓

Gateway

↓

Network

↓

Cloud

↓

Destination
```

Memorize this.

---

# Latency Is Also Packet Loss's Cousin

Packets may survive.

But slowly.

Causes:

```text
Congestion

Long routes

CPU overload

Buffering
```

Slow systems fail too.

---

# Why Congestion Happens

Imagine highways.

```text
10 cars

↓

Easy
```

```text
10 million cars

↓

Traffic jam
```

Networks behave similarly.

---

# Queueing Happens Everywhere

Examples:

```text
Routers

Switches

Load Balancers

Servers
```

Packets wait.

---

# Buffers Are Temporary Parking Lots

Visualization:

```text
Packets

↓

Buffer

↓

Forward
```

Too many packets:

```text
Buffer full

↓

Drop packets
```

---

# This Is Why Monitoring Exists

Engineers measure:

```text
Latency

Packet Loss

Jitter

Throughput

Errors
```

Constantly.

---

# Production Incident Example 1

Symptom:

```text
Website down.
```

Questions:

```text
DNS?

Network?

Application?
```

---

# Production Incident Example 2

Symptom:

```text
Only one region affected.
```

Questions:

```text
ISP?

Cloud?

CDN?
```

---

# Production Incident Example 3

Symptom:

```text
Pods unreachable.
```

Questions:

```text
CNI?

DNS?

Routes?
```

---

# The Universal Packet Death Algorithm ⭐⭐⭐⭐⭐

This is one of the most important diagrams.

```text
Application

↓

DNS

↓

Routing

↓

ARP

↓

Switch

↓

Router

↓

ISP

↓

Cloud

↓

Destination
```

Failure can occur anywhere.

---

# SRE Packet Hunting Framework ⭐⭐⭐⭐⭐

Question 1:

Was Penny born?

Application healthy?

---

Question 2:

Can Penny find destination?

DNS healthy?

---

Question 3:

Can Penny leave home?

Gateway healthy?

---

Question 4:

Can Penny travel?

Routing healthy?

---

Question 5:

Can Penny arrive?

Destination healthy?

---

Question 6:

Can Penny return?

Return path healthy?

---

# Linux Troubleshooting Commands

DNS:

```bash
dig google.com
```

Routes:

```bash
ip route
```

Neighbors:

```bash
ip neigh
```

Interfaces:

```bash
ip addr
```

Connections:

```bash
ss -tulpn
```

Packets:

```bash
tcpdump
```

Path:

```bash
traceroute google.com
```

---

# The Senior Engineer Mindset ⭐⭐⭐⭐⭐

Junior engineer says:

```text
Website down.
```

Senior engineer says:

```text
Where did the packet die?
```

Huge difference.

---

# Engineer Mental Models ⭐⭐⭐⭐⭐

# Mental Model 1

Do not think:

```text
Network down
```

Think:

```text
Packet stopped somewhere
```

---

# Mental Model 2

Do not think:

```text
Application issue
```

Think:

```text
Communication issue
```

---

# Mental Model 3

Do not think:

```text
Systems work
```

Think:

```text
Systems constantly fail
```

---

# Mental Model 4

Do not think:

```text
Failures are rare
```

Think:

```text
Failures are guaranteed
```

---

# Mental Model 5

Do not think:

```text
Distributed systems
```

Think:

```text
Thousands of opportunities to fail
```

---

# Common Misconceptions

### Misconception 1

"Packets rarely fail."

Wrong.

Packets constantly fail.

---

### Misconception 2

"Website down means server down."

Wrong.

Many layers exist.

---

### Misconception 3

"Cloud prevents failures."

Wrong.

Cloud relocates failures.

---

### Misconception 4

"Kubernetes is unstable."

Wrong.

Complex systems expose failures.

---

### Misconception 5

"Networking is reliable."

Wrong.

Networking is resilient.

Huge difference.

---

# WH Questions

## Why do packets get lost?

Failures everywhere.

---

## Why do retries exist?

Failures are normal.

---

## Why do timeouts exist?

Waiting forever is impossible.

---

## Why do experts follow packets?

Packets reveal failures.

---

## How do SREs think?

Find where communication stopped.

---

# Key Takeaways

✅ Packets die constantly

✅ Every hop is a failure opportunity

✅ Distributed systems multiply failures

✅ Retries are normal

✅ Timeouts are normal

✅ Senior engineers hunt packet deaths

✅ Production engineering is communication debugging

---

# Networking Internals Progress ⭐⭐⭐⭐⭐

```text
✓ 01-how-networks-were-born.md

✓ 02-how-a-packet-thinks.md

✓ 03-how-computers-talk.md

✓ 04-how-lan-actually-works.md

✓ 05-how-a-switch-thinks.md

✓ 06-how-a-router-thinks.md

✓ 07-how-a-packet-finds-its-destination.md

✓ 08-how-the-internet-actually-works.md

✓ 09-how-isps-work.md

✓ 10-how-google-receives-your-request.md

✓ 11-how-networks-scale.md

✓ 12-how-cloud-networking-is-built.md

✓ 13-how-kubernetes-becomes-a-network.md

✓ 14-how-packets-get-lost.md
```

# Repository Quality Note ⭐⭐⭐⭐⭐

The next file:

```text
15-how-network-failures-happen.md ⭐⭐⭐⭐⭐
```

is different.

This file will teach:

> Why systems fail architecturally.

Not individual packet failures.

This is where readers start thinking like senior SREs and infrastructure architects.
