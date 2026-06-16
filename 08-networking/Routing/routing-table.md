# Routing Tables

# Why This Matters

If routing is answering:

> "Where should this packet go?"

Then the routing table is the data structure Linux uses to answer it.

Every packet that leaves your machine eventually consults the routing table.

No exceptions.

Whether you're:

- Opening a website
- Calling an API
- Connecting to PostgreSQL
- Pulling Docker images
- Connecting Kubernetes Pods
- Accessing cloud services
- Building VPNs

Linux asks:

> Which route should I use?

Understanding routing tables is one of the biggest milestones in becoming a real infrastructure engineer.

---

# Learning Goals

After reading this file, you should be able to answer:

- What is a routing table?
- Why does Linux need one?
- How does Linux search it?
- How are routes selected?
- What is longest prefix matching?
- What are route metrics?
- What are route flags?
- How do cloud providers use route tables?
- How do Docker and Kubernetes use routes?
- How do engineers troubleshoot routing problems?

---

# The Big Picture

Applications do not know how to reach destinations.

Applications simply say:

```text
I want to talk to:

8.8.8.8
```

The kernel figures out the rest.

```text
Application

↓

Socket

↓

TCP/UDP

↓

IP Layer

↓

Routing Table Lookup

↓

Interface Selection

↓

Packet Transmission
```

The routing table is Linux's map.

---

# Think Like Google Maps

Imagine this.

You want to travel from:

```text
Delhi
```

to

```text
Mumbai
```

Google Maps consults:

```text
Road Database
```

Linux consults:

```text
Routing Table
```

Both answer:

```text
Which path should I take?
```

---

# What Is A Routing Table?

A routing table is:

> A collection of rules that tells Linux where packets should be sent.

Each rule is called a route.

Example:

```text
Destination      Next Hop       Interface

192.168.1.0/24   local          eth0

default          192.168.1.1    eth0
```

---

# Routing Table Components

Every route contains information.

```text
Destination

Next Hop

Interface

Metric

Protocol

Scope

Source Address
```

Not every route has every field.

But these are the common ones.

---

# Route Anatomy

Example:

```text
default via 192.168.1.1 dev eth0 metric 100
```

Break it down.

```text
default
```

Destination network.

---

```text
via 192.168.1.1
```

Gateway (next hop).

---

```text
dev eth0
```

Network interface.

---

```text
metric 100
```

Priority value.

Lower wins.

---

# Visual Example

```text
+-------------------------------------------------+
| Destination | Gateway     | Interface | Metric |
+-------------------------------------------------+
| 10.0.0.0/8  | local       | eth0      | 100    |
| 172.17/16   | local       | docker0   | 0      |
| default     |192.168.1.1  | eth0      | 100    |
+-------------------------------------------------+
```

---

# Viewing Routing Tables

Modern command:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0

192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.20

172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1
```

---

# Legacy Command

Older systems:

```bash
route -n
```

or

```bash
netstat -rn
```

Modern Linux prefers:

```bash
ip route
```

from:

```text
iproute2
```

---

# Reading A Route

Example:

```text
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.20
```

Meaning:

Destination:

```text
192.168.1.0/24
```

Use:

```text
eth0
```

Created by:

```text
kernel
```

Directly connected:

```text
scope link
```

Source IP:

```text
192.168.1.20
```

---

# Where Are Routing Tables Stored?

Beginners often imagine:

```text
/etc/routing-table
```

There is no such file.

Linux stores routes inside kernel memory.

User-space tools interact with it.

```text
ip command

↓

Netlink Socket

↓

Kernel Networking Stack

↓

FIB
```

---

# Meet The FIB

FIB = Forwarding Information Base

This is Linux's actual routing database.

Think:

```text
Routing Table = Human View

FIB = Kernel Working Database
```

---

# Linux Internals

When packets arrive:

Linux does NOT scan a text file.

Linux performs optimized lookups inside the FIB.

The kernel uses efficient data structures for speed.

Millions of packets per second may be processed.

Routing must be extremely fast.

---

# Packet Decision Flow

```text
Packet Created

↓

Extract Destination IP

↓

Kernel FIB Lookup

↓

Route Found?

↓

Yes → Select Interface

↓

Resolve MAC Address

↓

Transmit Packet
```

---

# Longest Prefix Match

This is the most important routing rule.

Linux always chooses:

> The most specific matching route.

Example:

Routing table:

```text
10.0.0.0/8

10.1.0.0/16

10.1.1.0/24

default
```

Destination:

```text
10.1.1.50
```

Linux chooses:

```text
10.1.1.0/24
```

---

# Visual Representation

```text
10.0.0.0/8

└──10.1.0.0/16

    └──10.1.1.0/24
```

The deepest match wins.

---

# Route Selection Algorithm

Linux roughly does:

```text
Take destination IP

↓

Find all matching routes

↓

Choose longest prefix

↓

If ties exist

↓

Choose lowest metric

↓

Use selected route
```

---

# What Is A Metric?

Metric = Route preference.

Smaller number wins.

Example:

```text
default via 192.168.1.1 metric 100

default via 10.0.0.1 metric 500
```

Linux chooses:

```text
192.168.1.1
```

because:

```text
100 < 500
```

---

# Why Multiple Default Routes Exist

Multi-homed servers.

Example:

```text
eth0 → Internet

eth1 → Backup ISP
```

Routing:

```text
default via ISP1 metric 100

default via ISP2 metric 500
```

If ISP1 fails:

Traffic switches to ISP2.

---

# Route Types

## Direct Route

No router needed.

```text
192.168.1.0/24 dev eth0
```

---

## Gateway Route

Needs a router.

```text
default via 192.168.1.1
```

---

## Blackhole Route

Discard traffic.

```bash
sudo ip route add blackhole 10.0.0.0/8
```

Used for:

- Security
- DDoS mitigation
- Traffic isolation

---

## Unreachable Route

Explicit failure.

```bash
sudo ip route add unreachable 10.20.0.0/16
```

Kernel returns:

```text
Network unreachable
```

---

# Route Sources

Routes can come from many places.

```text
Kernel

DHCP

Static configuration

VPN software

Docker

Kubernetes

Cloud systems

Dynamic routing protocols
```

---

# Route Protocol Field

Example:

```text
proto kernel
```

Means:

Kernel created it.

Other examples:

```text
proto static

proto dhcp

proto bgp
```

---

# Route Scope

Common values:

```text
scope host
```

Local machine only.

---

```text
scope link
```

Directly connected network.

---

```text
scope global
```

Reachable globally.

---

# Real Example

Server:

```text
192.168.1.20
```

Routes:

```text
192.168.1.0/24 dev eth0

default via 192.168.1.1 dev eth0
```

Request:

```text
8.8.8.8
```

Linux thinks:

```text
Is 8.8.8.8 inside 192.168.1.0/24?

No

Use default route.
```

---

# The Missing Step People Forget

Routing does NOT send packets directly.

After selecting a route:

Linux still needs:

```text
Destination MAC address
```

So:

```text
Routing

↓

ARP lookup

↓

Packet transmission
```

Routing and ARP work together.

---

# Complete Packet Journey

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Routing Table Lookup

↓

Gateway Selection

↓

ARP

↓

NIC

↓

Switch

↓

Router
```

---

# Docker Example

Container:

```text
172.17.0.2
```

Host:

```bash
ip route
```

Output:

```text
172.17.0.0/16 dev docker0
```

Meaning:

```text
Container traffic

↓

docker0 bridge

↓

Host

↓

Internet
```

Docker heavily depends on routing.

---

# Kubernetes Example

Cluster:

```text
Node A

Node B
```

Pod CIDRs:

```text
10.244.1.0/24

10.244.2.0/24
```

Node A routing table:

```text
10.244.2.0/24 via 192.168.10.22
```

Meaning:

```text
Pods on Node B

↓

Send traffic to Node B
```

This is why broken routes often break Kubernetes.

---

# AWS Example

VPC Route Table:

```text
10.0.0.0/16 local

0.0.0.0/0 igw
```

Meaning:

```text
Local traffic

↓

Stay inside VPC

Internet traffic

↓

Internet Gateway
```

Cloud networking is route management at enormous scale.

---

# Production Scenario 1

## Symptom

Cannot reach internet.

Check:

```bash
ip route
```

Output:

```text
192.168.1.0/24 dev eth0
```

Problem:

```text
No default route
```

Fix:

```bash
sudo ip route add default via 192.168.1.1
```

---

# Production Scenario 2

## Symptom

Kubernetes Pods cannot communicate.

Check:

```bash
ip route
```

Missing:

```text
10.244.x.x routes
```

Possible issue:

```text
Broken CNI
```

---

# Production Scenario 3

## Symptom

VPN connected but resources unavailable.

Investigate:

```bash
ip route
```

Problem:

```text
VPN routes not installed
```

---

# Troubleshooting Decision Tree

```text
Cannot Reach Destination

↓

Can I ping myself?

↓

Can I ping gateway?

↓

Check ip route

↓

Default route exists?

↓

No

↓

Add default route

↓

Yes

↓

Traceroute destination

↓

Find failing hop
```

---

# Essential Commands

View routes:

```bash
ip route
```

Detailed routes:

```bash
ip route show table all
```

Add route:

```bash
sudo ip route add 10.0.0.0/8 via 192.168.1.1
```

Delete route:

```bash
sudo ip route del 10.0.0.0/8
```

Replace route:

```bash
sudo ip route replace default via 192.168.1.1
```

Show rules:

```bash
ip rule
```

---

# Engineer Mental Model

Never think:

```text
My server talks to another server.
```

Think:

```text
Application

↓

Kernel

↓

Routing Table

↓

Gateway

↓

Switch

↓

Router

↓

Destination
```

This mindset changes how you debug systems.

---

# Security Considerations

Bad routes can expose infrastructure.

Examples:

Wrong default route.

Leaking private networks.

Routing internal databases to public networks.

Exposing Kubernetes APIs.

Always verify:

```text
Routes

Firewalls

Security Groups

NACLs

VPN Policies
```

Networking security starts with routing.

---

# Common Misconceptions

### Misconception 1

"The routing table is a file."

Wrong.

It's stored inside kernel memory.

---

### Misconception 2

"Applications decide routes."

Wrong.

Kernel decides routes.

---

### Misconception 3

"Default route is always internet."

Wrong.

It simply means:

```text
Catch-all route
```

---

### Misconception 4

"Only routers have routing tables."

Wrong.

Linux itself has one.

---

# WH Questions

## Why do routing tables exist?

To determine where packets should go.

---

## Who uses them?

Every Linux machine.

Every router.

Every cloud provider.

Every Kubernetes node.

---

## When are they used?

For every outgoing packet.

---

## Where are they stored?

Inside kernel memory.

---

## How are routes selected?

Longest prefix match.

Then lowest metric.

---

# Key Takeaways

✅ Every Linux machine has a routing table

✅ Linux stores routes inside kernel memory

✅ FIB powers routing decisions

✅ Longest prefix match is fundamental

✅ Metrics decide priorities

✅ Docker depends on routes

✅ Kubernetes depends heavily on routes

✅ Cloud networking is large-scale route management

✅ Troubleshooting networking almost always involves checking routes

---

# Recommended Supporting Files

```text
networking/Routing/

routing.md

routing-table.md

default-gateway.md

static-routing.md

dynamic-routing-introduction.md

internals.md

packet-journey.md

troubleshooting.md

security.md

comparisons.md

visuals.md
```

Recommended next file:

```text
networking/Routing/default-gateway.md
```
