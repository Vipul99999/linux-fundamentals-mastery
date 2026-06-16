# Story 09: How ISPs Work ⭐⭐⭐⭐⭐

# Why This File Exists

Every engineer uses an ISP.

Very few engineers understand what an ISP actually does.

People think:

```text
ISP

↓

Internet
```

Wrong.

ISPs do much more.

They:

- Build physical infrastructure
- Connect homes
- Connect businesses
- Connect cities
- Connect countries
- Buy internet access from other networks
- Exchange traffic with giant companies

Without ISPs, the internet would not exist.

This file teaches how ISPs actually work.

---

# Learning Goals

After reading this file, you should be able to answer:

- What is an ISP?
- Why do ISPs exist?
- What infrastructure do ISPs build?
- What happens after buying internet?
- How do packets leave your home?
- How do ISPs connect to each other?
- What is peering?
- What is transit?
- Why do production incidents involve ISPs?

---

# The Story Begins

Imagine.

You buy an internet connection.

Your house:

```text
Laptop

↓

WiFi Router
```

Question:

Who connects your house to Google?

Your ISP.

---

# ISP Definition

ISP means:

> Internet Service Provider

But that definition is too simple.

A better definition:

> An organization that connects your network to other networks.

That's the real job.

---

# Think Of Roads

Imagine your house.

```text
House

↓

Local Road

↓

Highway

↓

National Highway

↓

Destination
```

ISPs build networking highways.

---

# Your Home Is Not The Internet

This is one of the biggest misconceptions.

Wrong.

```text
Laptop

↓

Internet
```

Correct.

```text
Laptop

↓

Home Network

↓

ISP

↓

Internet
```

Huge difference.

---

# Meet Ivan The ISP

Ivan owns infrastructure.

Ivan owns:

```text
Fiber

Routers

Switches

Data Centers

POP Sites

Edge Routers
```

Ivan's job:

```text
Move packets.
```

---

# What Happens When You Buy Internet?

Your ISP creates a path.

```text
Home

↓

Neighborhood

↓

City

↓

Regional Network

↓

National Network

↓

Global Internet
```

---

# The Entire Journey ⭐⭐⭐⭐⭐

```text
Laptop

↓

WiFi Router

↓

ISP Access Network

↓

ISP Aggregation Network

↓

ISP Core Network

↓

Transit Network

↓

Destination Network

↓

Google
```

This is a huge concept.

---

# ISP Architecture

Most ISPs look like this.

```text
Customer

↓

Access Layer

↓

Aggregation Layer

↓

Core Layer

↓

Internet
```

---

# Layer 1

# Access Network

This is closest to users.

Examples:

```text
Fiber

Cable

DSL

5G

Wireless Towers
```

Purpose:

```text
Connect customers
```

---

# Layer 2

# Aggregation Network

Aggregation means:

```text
Combine many neighborhoods
```

Visualization:

```text
100 Homes

↓

Aggregation Router
```

---

# Layer 3

# Core Network

Core means:

```text
High-speed backbone
```

This moves enormous traffic.

Visualization:

```text
Cities

↓

Core Routers

↓

Countries
```

---

# The ISP Is Basically A Giant Router Company

This mental model is extremely useful.

ISPs are giant routing systems.

---

# Home Example

Suppose:

```text
192.168.1.20
```

opens Google.

Journey:

```text
Laptop

↓

Home Router

↓

Airtel Access Router

↓

Airtel Aggregation Router

↓

Airtel Core Router

↓

Transit Provider

↓

Google
```

---

# ISPs Don't Own The Entire Internet

This is critical.

ISPs cooperate.

There are two major ways.

```text
Peering

Transit
```

---

# Peering ⭐⭐⭐⭐⭐

Peering means:

```text
I help your customers.

You help my customers.
```

Free or mutually beneficial.

Visualization:

```text
ISP A

↔

ISP B
```

Equal relationship.

---

# Transit ⭐⭐⭐⭐⭐

Transit means:

```text
Pay me.

I'll connect you to the world.
```

Visualization:

```text
Small ISP

↓

Big ISP

↓

Internet
```

Business relationship.

---

# Peering vs Transit

| Peering | Transit |
|---------|---------|
| Partnership | Customer relationship |
| Often free | Paid |
| Equal networks | Hierarchical |
| Traffic exchange | Internet access |

This distinction is extremely important.

---

# Internet Exchange Points (IXP)

Humans built meeting places.

```text
ISP A

↓

IXP

↓

ISP B
```

Purpose:

```text
Exchange traffic efficiently.
```

Examples exist worldwide.

---

# Why ISPs Build POPs

POP:

```text
Point Of Presence
```

Think:

```text
Mini data centers near customers.
```

Purpose:

```text
Reduce latency.
```

---

# Visualization

```text
Home

↓

Local POP

↓

City POP

↓

Core Network
```

---

# ISPs Hate Long Routes

Long routes increase:

```text
Latency

Cost

Congestion
```

ISPs constantly optimize paths.

---

# The Economics Of The Internet

The internet is also business.

Questions ISPs ask:

```text
Cheapest route?

Fastest route?

Reliable route?
```

All simultaneously.

---

# Why Sometimes YouTube Is Slow

Many possibilities.

```text
Congested ISP

Bad Peering

Congested Transit

CDN Issue
```

The application may be healthy.

The network isn't.

---

# Why Sometimes Websites Work In One Country Only

Example:

```text
India broken

Europe works
```

Potential causes:

```text
ISP issue

Peering issue

BGP issue
```

---

# Undersea Cables Matter

ISPs connect countries physically.

Visualization:

```text
India

↓

Ocean Cable

↓

Middle East

↓

Europe

↓

USA
```

Fiber powers the world.

---

# Why Large Companies Avoid Public Internet

Public internet is unpredictable.

Companies build private backbones.

Examples:

```text
Google

AWS

Microsoft

Meta
```

Private highways.

---

# Google Network Example

Instead of:

```text
Public Internet
```

Google prefers:

```text
Google Fiber

↓

Google Routers

↓

Google Data Centers
```

Faster.

More reliable.

---

# Cloud Providers Are Giant ISPs

This is a powerful mental model.

AWS is essentially:

```text
Cloud

+

Global ISP

+

Data Center Company
```

Same for:

```text
Azure

Google Cloud
```

---

# Kubernetes Eventually Uses ISPs Too

Pod communication.

```text
Pod

↓

Node

↓

Cloud Network

↓

ISP

↓

Destination
```

Eventually everything reaches ISP infrastructure.

---

# Production Incident Example 1

Symptom:

```text
Website unreachable only from one ISP.
```

Question:

```text
ISP issue?
```

Very common.

---

# Production Incident Example 2

Symptom:

```text
High latency only in one region.
```

Question:

```text
Peering issue?
```

---

# Production Incident Example 3

Symptom:

```text
Application healthy

Users cannot access it
```

Question:

```text
Transit issue?
```

---

# Production Troubleshooting Workflow ⭐⭐⭐⭐⭐

Question 1:

Local network healthy?

---

Question 2:

Gateway healthy?

---

Question 3:

ISP reachable?

---

Question 4:

Traceroute healthy?

---

Question 5:

Regional issue?

---

Question 6:

Global issue?

---

# Useful Tools

Traceroute:

```bash
traceroute google.com
```

DNS:

```bash
dig google.com
```

Packets:

```bash
tcpdump
```

Routes:

```bash
ip route
```

---

# Universal ISP Algorithm ⭐⭐⭐⭐⭐

Every ISP on Earth essentially does this.

```text
Receive Customer Traffic

↓

Aggregate Customers

↓

Move Through Backbone

↓

Exchange Traffic

↓

Deliver Traffic
```

That's an ISP.

---

# Engineer Mental Models ⭐⭐⭐⭐⭐

# Mental Model 1

Do not think:

```text
ISP = Internet Company
```

Think:

```text
ISP = Network Connector
```

---

# Mental Model 2

Do not think:

```text
Internet
```

Think:

```text
Businesses cooperating
```

---

# Mental Model 3

Do not think:

```text
Website is down
```

Think:

```text
Which network failed?
```

---

# Mental Model 4

Do not think:

```text
Cloud provider
```

Think:

```text
Massive ISP + Data Center
```

---

# Common Misconceptions

### Misconception 1

"ISP is the internet."

Wrong.

ISP connects you to the internet.

---

### Misconception 2

"ISPs know every device."

Wrong.

ISPs know routes.

---

### Misconception 3

"The internet is free."

Wrong.

It's expensive infrastructure.

---

### Misconception 4

"Cloud replaced ISPs."

Wrong.

Cloud providers depend on ISP concepts.

---

### Misconception 5

"Internet is mostly wireless."

Wrong.

Mostly fiber.

---

# WH Questions

## Why do ISPs exist?

To connect networks.

---

## Why do ISPs peer?

To exchange traffic efficiently.

---

## Why does transit exist?

To scale globally.

---

## Why do IXPs exist?

To reduce costs and latency.

---

## How do experts think?

As interconnected businesses moving packets.

---

# Key Takeaways

✅ ISPs connect networks

✅ ISPs build physical infrastructure

✅ Peering and transit power the internet

✅ IXPs improve efficiency

✅ Undersea cables matter

✅ Cloud providers build ISP-like networks

✅ Many production issues are network issues

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
```

# Next File ⭐⭐⭐⭐⭐

```text
networking-internals/

10-how-google-receives-your-request.md ⭐⭐⭐⭐⭐
```

This file will be one of the **highest-value files in the entire repository**.

Because it will answer:

> After packets cross the internet, what actually happens inside Google?

That file will connect:

```text
CDN

Anycast

Edge Routers

Load Balancers

Data Centers

Linux Servers

Distributed Systems
```

into one unified story.
