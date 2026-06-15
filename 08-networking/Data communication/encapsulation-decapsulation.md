# Encapsulation & Decapsulation

> Learn how data actually travels through networks and how Linux transforms application data into packets that can traverse the internet.

---

# Why Learn This?

This file answers one of the most important networking questions.

When you type:

https://google.com

How does that simple request become network traffic?

The answer is:

Encapsulation.

At the destination, the reverse process happens.

The answer is:

Decapsulation.

Without understanding this file:

❌ TCP is confusing

❌ IP is confusing

❌ DNS is confusing

❌ Routing is confusing

❌ Linux networking internals are confusing

---

# Simple Definition

## Encapsulation

The process of wrapping data with additional information while moving down the networking stack.

---

## Decapsulation

The process of removing those layers while moving up the networking stack.

---

# Simple Analogy

Imagine sending a gift.

---

Original gift

```text
Laptop Charger
```

---

Add a box

```text
[Box]

Laptop Charger
```

---

Add delivery address

```text
[Delivery Address]

[Box]

Laptop Charger
```

---

Add shipping label

```text
[Shipping Label]

[Delivery Address]

[Box]

Laptop Charger
```

Networking does exactly this.

---

# Big Picture

```text
Application Data

↓

Transport Layer

↓

Internet Layer

↓

Data Link Layer

↓

Physical Layer
```

At the destination:

```text
Physical Layer

↓

Data Link Layer

↓

Internet Layer

↓

Transport Layer

↓

Application Layer
```

---

# Why Does Encapsulation Exist?

Because every layer has its own responsibility.

Example:

Transport layer:

```text
How do applications communicate?
```

Internet layer:

```text
Where should this go?
```

Data link layer:

```text
Who in this local network should receive this?
```

Physical layer:

```text
How do we physically transmit it?
```

---

# Complete Visual

```text
Application Data

↓

TCP Header

↓

IP Header

↓

Ethernet Header

↓

Bits
```

---

# Example Scenario

You open:

```text
https://google.com
```

Browser creates:

```text
GET /
```

This is application data.

---

# Step 1: Application Layer

Creates:

```text
HTTP Request
```

Example:

```text
GET /

Host: google.com
```

Current state:

```text
HTTP Data
```

---

# Step 2: Transport Layer

TCP adds its information.

Adds:

```text
Source Port

Destination Port

Sequence Number

Acknowledgement Number

Flags
```

Now:

```text
[TCP Header]

[HTTP Data]
```

This is called:

```text
Segment
```

---

# Step 3: Internet Layer

IP adds information.

Adds:

```text
Source IP

Destination IP

TTL

Protocol
```

Now:

```text
[IP Header]

[TCP Header]

[HTTP Data]
```

This becomes:

```text
Packet
```

---

# Step 4: Data Link Layer

Ethernet adds information.

Adds:

```text
Source MAC

Destination MAC

Frame Check Sequence
```

Now:

```text
[Ethernet Header]

[IP Header]

[TCP Header]

[HTTP Data]
```

This becomes:

```text
Frame
```

---

# Step 5: Physical Layer

Everything becomes bits.

```text
1010101010
```

Transmitted via:

```text
Cable

WiFi

Fiber
```

---

# Data Unit Names

| Layer | Name |
|------|------|
| Application | Data |
| Transport | Segment |
| Internet | Packet |
| Data Link | Frame |
| Physical | Bits |

---

# Full Encapsulation Visualization

```text
Application Data

↓

[TCP Header]

Application Data

↓

[IP Header]

[TCP Header]

Application Data

↓

[Ethernet Header]

[IP Header]

[TCP Header]

Application Data

↓

Bits
```

---

# Decapsulation

Destination performs reverse operations.

---

# Step 1

Receive bits.

```text
Bits
```

---

# Step 2

Remove Ethernet information.

```text
Ethernet Header

↓

Removed
```

---

# Step 3

Remove IP information.

```text
IP Header

↓

Removed
```

---

# Step 4

Remove TCP information.

```text
TCP Header

↓

Removed
```

---

# Step 5

Application receives data.

```text
HTTP Request
```

---

# End To End Journey

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

WiFi

↓

Internet

↓

Google

↓

Ethernet

↓

IP

↓

TCP

↓

HTTP

↓

Google Application
```

---

# Linux Internals ⭐⭐⭐

This is where Linux becomes interesting.

Linux internally performs:

```text
Application

↓

Socket API

↓

Kernel Space

↓

TCP Layer

↓

IP Layer

↓

ARP

↓

Network Driver

↓

NIC Hardware
```

---

# Linux Packet Journey

```text
Browser

↓

Socket()

↓

Kernel

↓

TCP

↓

IP

↓

Routing Table

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

# Internal Linux Components

| Component | Responsibility |
|-----------|---------------|
| Socket API | Application communication |
| TCP | Reliability |
| IP | Addressing |
| Routing Table | Path selection |
| ARP | MAC discovery |
| NIC Driver | Hardware communication |
| NIC | Physical transmission |

---

# Internal Header Visualization

```text
┌─────────────────────────┐
│ Ethernet Header         │
├─────────────────────────┤
│ IP Header               │
├─────────────────────────┤
│ TCP Header              │
├─────────────────────────┤
│ Application Data        │
└─────────────────────────┘
```

---

# Why Engineers Care About This

Because problems occur at different layers.

Examples:

```text
HTTP issue

↓

Application
```

```text
TCP timeout

↓

Transport
```

```text
Routing issue

↓

Internet
```

```text
ARP issue

↓

Data Link
```

---

# Real Production Example

Suppose users cannot access:

```text
https://github.com
```

Possible failures:

```text
DNS

↓

TCP

↓

Routing

↓

Firewall

↓

ARP

↓

NIC
```

Understanding encapsulation helps isolate failures.

---

# Modern Infrastructure Usage

Cloud:

```text
AWS

Azure

GCP
```

Containers:

```text
Docker

Kubernetes
```

Microservices:

```text
Service A

↓

Service B

↓

Database
```

All depend on encapsulation.

---

# Linux Engineer Mindset

Never think:

```text
Request

↓

Server
```

Think:

```text
Application

↓

TCP

↓

IP

↓

Ethernet

↓

Internet

↓

Destination
```

---

# Troubleshooting Mindset

Ask:

```text
Where did encapsulation break?

↓

Application?

↓

TCP?

↓

IP?

↓

Ethernet?

↓

Hardware?
```

---

# WH Questions

## What is encapsulation?

Wrapping data with networking information.

---

## Why does encapsulation exist?

Every layer has different responsibilities.

---

## What is decapsulation?

Removing those layers.

---

## Why are headers added?

To provide instructions for communication.

---

## Why is this important?

Because every network communication depends on it.

---

# Key Takeaways

Remember this forever.

```text
Application Data

↓

Segment

↓

Packet

↓

Frame

↓

Bits
```

---

# What's Next?

```text
packet-vs-frame-vs-segment.md
```

This file will deeply explain the differences because many engineers confuse these terms.
