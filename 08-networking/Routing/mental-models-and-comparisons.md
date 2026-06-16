# Routing Comparisons & Engineer Mental Models

# Why This File Exists

There is a point where engineers stop learning individual technologies.

And start connecting technologies together.

This file exists to build those connections.

This is one of the highest ROI files in this entire section.

The goal is not memorization.

The goal is intuition.

If you understand these comparisons, troubleshooting becomes dramatically easier.

---

# Learning Goals

After reading this file, you should be able to answer:

- When should I use static routing?
- When should I use dynamic routing?
- How is a gateway different from a router?
- How is ARP different from routing?
- How is Layer 2 different from Layer 3?
- How does Docker networking compare to Kubernetes networking?
- How does cloud networking compare to traditional networking?
- How do Linux engineers think about networking?

---

# The Master Mental Model

Do not memorize technologies.

Think in questions.

Every packet asks:

```text
Who am I?

↓

Where am I going?

↓

How do I get there?

↓

Who should I send it to?

↓

Can I physically reach them?

↓

Can they send a response back?
```

Networking is simply answering these questions repeatedly.

---

# Comparison 1

# Router vs Gateway

This confuses almost everyone.

---

## Router

Definition:

```text
A device that forwards packets between networks.
```

Question it answers:

```text
Where should this packet go next?
```

Example:

```text
Office Router

ISP Router

AWS Virtual Router
```

---

## Gateway

Definition:

```text
The next router your machine uses.
```

Question it answers:

```text
Who should I ask when I don't know where to go?
```

---

# Visualization

```text
Laptop

↓

Default Gateway

↓

Router

↓

Router

↓

Internet
```

---

# Key Insight

Every gateway is a router.

Not every router is your gateway.

---

# Comparison Table

| Gateway | Router |
|---------|--------|
| Local machine perspective | Network perspective |
| Usually first hop | Can be any hop |
| A routing table entry | A forwarding device |
| Used by hosts | Used by infrastructure |

---

# Comparison 2

# ARP vs Routing

This is one of the biggest beginner mistakes.

---

# Routing Answers

```text
Where should packet go?
```

---

# ARP Answers

```text
Who owns this IP?
```

---

# Visualization

```text
Routing

↓

Choose gateway

↓

ARP

↓

Find MAC
```

---

# Example

Destination:

```text
8.8.8.8
```

Routing:

```text
Use 192.168.1.1
```

ARP:

```text
Who owns 192.168.1.1?
```

---

# Engineer Rule

Routing always happens before ARP.

---

# Comparison Table

| Routing | ARP |
|---------|-----|
| Layer 3 | Layer 2 |
| IP focused | MAC focused |
| Chooses path | Finds device |
| Uses FIB | Uses ARP cache |

---

# Comparison 3

# FIB vs Routing Table

Many engineers incorrectly use these interchangeably.

---

# Routing Table

Human view.

```bash
ip route
```

---

# FIB

Kernel forwarding database.

Linux actually uses this.

---

# Visualization

```text
ip route

↓

Netlink

↓

FIB

↓

Packet forwarding
```

---

# Comparison Table

| Routing Table | FIB |
|--------------|-----|
| Human readable | Kernel optimized |
| Viewed with commands | Used internally |
| Administrative view | Runtime execution |

---

# Comparison 4

# FIB vs RIB

Professional networking concept.

---

# RIB

Knowledge database.

Stores learned routes.

---

# FIB

Execution database.

Stores forwarding routes.

---

# Visualization

```text
OSPF/BGP

↓

RIB

↓

FIB

↓

Packet Forwarding
```

---

# Comparison Table

| RIB | FIB |
|-----|-----|
| Knowledge | Execution |
| Large | Optimized |
| Dynamic routing uses it | Kernel uses it |

---

# Comparison 5

# Static Routing vs Dynamic Routing

---

# Static Routing

Humans teach routes.

---

# Dynamic Routing

Routers teach each other.

---

# Visualization

Static:

```text
Engineer

↓

Router
```

Dynamic:

```text
Router

↓

Router

↓

Router
```

---

# Comparison Table

| Static | Dynamic |
|--------|---------|
| Manual | Automatic |
| Simple | Complex |
| Small scale | Large scale |
| Low CPU | Higher CPU |
| Low flexibility | High flexibility |

---

# When To Use Static

Use when:

```text
Small office

VPN

Cloud routes

Backup routes

Simple infrastructure
```

---

# When To Use Dynamic

Use when:

```text
Data centers

ISPs

Large Kubernetes clusters

Multi-region systems

Huge enterprises
```

---

# Comparison 6

# Layer 2 vs Layer 3

---

# Layer 2

Moves frames.

Uses:

```text
MAC addresses
```

Devices:

```text
Switches
```

---

# Layer 3

Moves packets.

Uses:

```text
IP addresses
```

Devices:

```text
Routers
```

---

# Visualization

```text
Layer 2

Room Navigation

Layer 3

City Navigation
```

---

# Comparison Table

| Layer 2 | Layer 3 |
|---------|---------|
| MAC | IP |
| Frames | Packets |
| Switches | Routers |
| Local movement | Cross-network movement |

---

# Comparison 7

# DNS vs Routing

Another huge misconception.

---

# DNS

Name resolution.

```text
google.com

↓

142.250.x.x
```

---

# Routing

Path resolution.

```text
142.250.x.x

↓

Gateway

↓

Internet
```

---

# Visualization

```text
DNS

↓

What address?

↓

Routing

↓

How to get there?
```

---

# Comparison Table

| DNS | Routing |
|-----|---------|
| Name → IP | IP → Path |
| Application layer | Network layer |
| Naming | Movement |

---

# Comparison 8

# Local Network vs Remote Network

---

# Local

```text
192.168.1.20

↓

192.168.1.50
```

No router.

---

# Remote

```text
192.168.1.20

↓

8.8.8.8
```

Router required.

---

# Comparison Table

| Local | Remote |
|-------|--------|
| Direct communication | Router needed |
| Same subnet | Different subnet |

---

# Comparison 9

# Docker Networking vs Kubernetes Networking

This is a very important engineering comparison.

---

# Docker

Goal:

```text
Container

↓

Host

↓

Internet
```

Simple.

---

# Kubernetes

Goal:

```text
Pod

↓

Node

↓

Other Nodes

↓

Cluster
```

Distributed.

---

# Complexity Comparison

Docker:

```text
Single machine networking
```

Kubernetes:

```text
Distributed networking
```

---

# Comparison Table

| Docker | Kubernetes |
|--------|------------|
| Single host | Multi-host |
| Simple routes | Distributed routes |
| docker0 | CNI |
| Easier | Harder |

---

# Comparison 10

# Traditional Networking vs Cloud Networking

This is extremely important.

---

# Traditional

Physical devices.

```text
Router

Switch

Firewall
```

---

# Cloud

Software versions.

```text
Virtual Router

Virtual Switch

Virtual Firewall
```

---

# The Secret

Cloud did not invent networking.

Cloud automated networking.

---

# Visualization

Traditional:

```text
Cable

↓

Router

↓

Switch
```

Cloud:

```text
Software

↓

Virtual Router

↓

Virtual Switch
```

---

# Comparison Table

| Traditional | Cloud |
|-------------|-------|
| Physical | Virtual |
| Hardware | Software |
| Manual | Automated |

---

# Comparison 11

# VPN vs Default Gateway

---

# Default Gateway

Catch-all route.

---

# VPN

Specialized route path.

---

# Visualization

Default:

```text
Internet

↓

ISP
```

VPN:

```text
Internet

↓

Encrypted Tunnel
```

---

# Engineer Mental Models

# Mental Model 1

Do not think:

```text
Computer

↓

Internet
```

Think:

```text
Application

↓

Kernel

↓

Route

↓

ARP

↓

NIC

↓

Switch

↓

Router

↓

Internet
```

---

# Mental Model 2

Do not think:

```text
Networking
```

Think:

```text
Packet Movement
```

---

# Mental Model 3

Do not think:

```text
Service unreachable
```

Think:

```text
Where did packet stop?
```

---

# Mental Model 4

Do not think:

```text
Cloud networking
```

Think:

```text
Linux networking at massive scale
```

---

# The Golden Troubleshooting Formula

Always ask:

```text
Who am I?

↓

Where am I going?

↓

Do I have a route?

↓

Can I reach next hop?

↓

Can they return traffic?
```

This formula scales from:

```text
Laptop

↓

Docker

↓

Kubernetes

↓

AWS

↓

Google-scale infrastructure
```

---

# Common Misconceptions

### Misconception 1

"Cloud networking is different."

Wrong.

Same concepts.

Different implementation.

---

### Misconception 2

"Kubernetes invented networking."

Wrong.

Kubernetes automates Linux networking.

---

### Misconception 3

"Gateway and router are identical."

Partially wrong.

Perspective matters.

---

### Misconception 4

"DNS is networking."

Wrong.

DNS supports networking.

---

# Key Takeaways

✅ Learn relationships, not isolated technologies

✅ ARP and routing are different

✅ Gateways and routers are perspective-dependent

✅ Docker and Kubernetes are Linux networking automation

✅ Cloud networking is virtual networking

✅ Every packet asks the same questions

---

# Routing Section Status

Current Repository:

```text
networking/Routing/

✅ routing.md

✅ routing-table.md

✅ default-gateway.md

✅ static-routing.md

✅ dynamic-routing-introduction.md

✅ packet-journey.md

✅ internals.md

✅ troubleshooting.md

✅ security.md

✅ comparisons.md
```

# Repository Evolution Recommendation

At this point, **Routing is now sufficiently complete**.

Instead of endlessly adding more files, I would now expand horizontally.

The next top-level repository section should be:

```text
networking/Protocols/
```

with:

```text
networking/Protocols/

routing-protocols-overview.md

rip.md

ospf.md

bgp.md

isis.md

eigrp.md
```

**Do not immediately dive into BGP.**

Create:

```text
networking/Protocols/routing-protocols-overview.md
```

first.

That will build a proper foundation for everything that follows.
