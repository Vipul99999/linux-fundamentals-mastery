# Routing Internals

# Why This File Exists

Most networking courses stop here:

```text
Application

↓

Routing Table

↓

Gateway

↓

Internet
```

That is useful.

But it is not how Linux actually works.

Production engineers eventually discover:

```text
There is no magic.
```

Linux is simply many subsystems working together.

This file moves from:

```text
Linux User
```

to

```text
Linux Engineer
```

We are going behind the scenes.

We'll answer:

> How does Linux actually route packets internally?

This knowledge is invaluable for:

- Linux Engineers
- Backend Engineers
- DevOps Engineers
- SREs
- Platform Engineers
- Cloud Engineers

---

# Learning Goals

After reading this file, you should be able to answer:

- What actually happens when Linux routes a packet?
- What is the FIB?
- What is the RIB?
- What is Netlink?
- What is the Neighbor subsystem?
- What are routing rules?
- What is policy routing?
- How do Docker and Kubernetes interact with routing internals?
- How do engineers debug kernel networking?

---

# The Big Picture

Applications do not control networking.

The Linux kernel does.

Real flow:

```text
Application

↓

System Call

↓

Socket Layer

↓

Transport Layer

↓

IP Layer

↓

Routing Lookup

↓

Neighbor Lookup

↓

Traffic Control

↓

Network Driver

↓

NIC
```

Linux orchestrates all of this.

---

# High Level Linux Networking Architecture

```text
+----------------+

Applications

+----------------+

↓

+----------------+

Sockets

+----------------+

↓

+----------------+

TCP / UDP

+----------------+

↓

+----------------+

IP Layer

+----------------+

↓

+----------------+

Routing Subsystem

+----------------+

↓

+----------------+

Neighbor Subsystem

+----------------+

↓

+----------------+

Traffic Control

+----------------+

↓

+----------------+

NIC Driver

+----------------+

```

---

# Kernel Networking Directory

If you explore Linux source code:

```text
linux/

net/
```

Important directories:

```text
net/core

net/ipv4

net/ipv6

net/sched

net/bridge

net/netfilter
```

These power Linux networking.

---

# What Happens When An Application Sends Data?

Application:

```python
requests.get("https://api.github.com")
```

Kernel sequence:

```text
Application

↓

syscall()

↓

Socket

↓

TCP

↓

IP

↓

Routing Lookup

↓

ARP

↓

NIC

↓

Wire
```

Millions of times per second.

---

# System Calls

Applications cannot directly touch hardware.

They ask the kernel.

Example:

```c
connect()
```

Kernel takes over.

---

# The Socket Layer

Sockets are communication endpoints.

Think:

```text
Application Doorway
```

Applications say:

```text
Connect to:

140.82.121.5
```

Socket layer forwards request.

---

# The Transport Layer

TCP adds:

```text
Ports

Sequence Numbers

ACK Numbers

Flow Control
```

Packet becomes:

```text
TCP Segment
```

---

# The IP Layer

Linux wraps TCP inside IP.

Adds:

```text
Source IP

Destination IP

TTL

Protocol
```

Packet becomes:

```text
IP Packet
```

Now Linux must answer:

```text
How do I reach destination?
```

This is where internals begin.

---

# Meet The FIB

FIB:

> Forwarding Information Base

This is Linux's real routing database.

Most beginners think:

```text
ip route
```

is the routing table.

Not exactly.

`ip route` is simply a view.

Linux internally uses:

```text
FIB
```

---

# Think Of It Like This

```text
ip route

↓

Human-readable view

↓

Kernel FIB
```

The FIB performs actual lookups.

---

# FIB Responsibilities

The FIB answers:

```text
Destination IP

↓

Which route?

↓

Which interface?

↓

Which gateway?
```

Millions of times per second.

It must be extremely efficient.

---

# FIB Lookup Visualization

```text
Packet

↓

Destination IP

↓

FIB Lookup

↓

Route Found

↓

Interface Selected

↓

Continue
```

---

# The RIB vs FIB

This is a professional networking concept.

---

## RIB

Routing Information Base

Stores all routing information.

Think:

```text
Knowledge Database
```

---

## FIB

Forwarding Information Base

Stores optimized forwarding routes.

Think:

```text
Execution Database
```

---

# Visualization

```text
Dynamic Routing Protocol

↓

RIB

↓

FIB

↓

Packet Forwarding
```

---

# Important Note

Linux itself mostly exposes FIB concepts.

But when using:

```text
FRRouting

Bird

BGP

OSPF
```

The RIB becomes important.

---

# The Neighbor Subsystem

Routing only finds:

```text
Where to go
```

Linux still needs:

```text
Who owns this IP?
```

Neighbor subsystem solves this.

For IPv4:

```text
ARP
```

For IPv6:

```text
NDP
```

---

# Neighbor Lookup Flow

```text
Route Found

↓

Next Hop IP

↓

Neighbor Lookup

↓

MAC Found?

↓

Yes

↓

Transmit
```

---

# View Neighbor Cache

```bash
ip neigh
```

Example:

```text
192.168.1.1 lladdr aa:bb:cc dev eth0 REACHABLE
```

---

# Neighbor States

Linux tracks state.

Common values:

```text
INCOMPLETE

REACHABLE

STALE

DELAY

PROBE

FAILED
```

---

# Visualization

```text
INCOMPLETE

↓

REACHABLE

↓

STALE

↓

DELAY

↓

PROBE
```

---

# What Is Netlink?

Netlink is one of Linux's most important technologies.

Netlink is:

> Communication channel between user space and kernel space.

---

# Visualization

```text
ip command

↓

Netlink Socket

↓

Kernel

↓

FIB
```

---

# Examples

This command:

```bash
ip route add 10.0.0.0/8 via 192.168.1.1
```

Actually does:

```text
ip tool

↓

Netlink

↓

Kernel

↓

FIB Updated
```

---

# Why Netlink Exists

Without it:

```text
Applications

↓

Cannot modify networking state
```

Everything networking uses Netlink.

Examples:

```text
Docker

Kubernetes

FRRouting

NetworkManager

systemd-networkd

Calico
```

---

# Policy Routing

So far we've learned:

```text
One routing table
```

Large infrastructures need more.

Linux supports:

```text
Multiple routing tables
```

This is:

> Policy Routing

---

# Example Problem

Suppose:

```text
eth0

Internet

eth1

VPN
```

Requirement:

```text
Company traffic

↓

VPN

Internet traffic

↓

ISP
```

One table isn't enough.

Policy routing solves this.

---

# Policy Routing Architecture

```text
Packet

↓

Routing Rule

↓

Table Selection

↓

Route Lookup

↓

Forward
```

---

# Routing Rules

View:

```bash
ip rule
```

Example:

```text
0: from all lookup local

32766: from all lookup main

32767: from all lookup default
```

Linux checks rules top to bottom.

---

# Policy Routing Example

Table:

```text
100 vpn
```

Add route:

```bash
ip route add default via 10.8.0.1 table 100
```

Add rule:

```bash
ip rule add from 192.168.100.0/24 table 100
```

Meaning:

```text
Traffic from:

192.168.100.x

↓

VPN
```

---

# Linux Packet Processing Pipeline

This is one of the most important diagrams.

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Routing Rules

↓

FIB

↓

Neighbor Lookup

↓

Traffic Control

↓

NIC Driver

↓

NIC
```

Memorize this.

---

# What Is Traffic Control?

Traffic control:

```text
tc
```

Allows:

```text
Rate limiting

Bandwidth shaping

Prioritization

Queue management
```

Production engineers use this often.

---

# Example

Limit bandwidth:

```bash
tc qdisc add dev eth0 root tbf rate 100mbit burst 32k latency 400ms
```

---

# Docker Internals

Docker heavily interacts with Linux networking internals.

Creates:

```text
Namespaces

veth pairs

Bridges

Routes

iptables rules
```

---

# Docker Visualization

```text
Container

↓

Namespace

↓

veth

↓

docker0

↓

Host Kernel

↓

eth0
```

---

# Kubernetes Internals

Kubernetes is Linux networking automation.

Creates:

```text
Namespaces

veth

Routes

iptables

eBPF

BGP
```

Depending on CNI.

---

# Kubernetes Visualization

```text
Pod

↓

Namespace

↓

veth

↓

Bridge

↓

Routing

↓

Other Node
```

---

# Cloud Networking Internals

Cloud providers virtualize everything.

Real architecture:

```text
VM

↓

Virtual NIC

↓

Hypervisor

↓

Virtual Switch

↓

Virtual Router

↓

Physical Network
```

Networking becomes software.

---

# Production Scenario 1

## Symptom

Container has no internet.

Investigate:

```bash
ip route

ip addr

iptables

sysctl net.ipv4.ip_forward
```

---

# Production Scenario 2

## Symptom

VPN connected.

Traffic still uses ISP.

Check:

```bash
ip rule

ip route
```

Likely policy routing issue.

---

# Production Scenario 3

## Symptom

Kubernetes Pods unreachable.

Check:

```bash
ip route

ip neigh

iptables

CNI logs
```

---

# Production Scenario 4

## Symptom

Intermittent network failures.

Check:

```bash
ip neigh
```

State:

```text
FAILED
```

Likely ARP issue.

---

# Troubleshooting Workflow

Question 1:

Do I have an IP?

```bash
ip addr
```

---

Question 2:

Do I have a route?

```bash
ip route
```

---

Question 3:

Which rule is applied?

```bash
ip rule
```

---

Question 4:

Can I resolve neighbors?

```bash
ip neigh
```

---

Question 5:

Can I see packet path?

```bash
traceroute
```

---

# Troubleshooting Decision Tree

```text
Packet Failure

↓

IP Exists?

↓

No

↓

Fix Interface

↓

Yes

↓

Route Exists?

↓

No

↓

Fix Route

↓

Yes

↓

Neighbor Exists?

↓

No

↓

Fix ARP

↓

Yes

↓

Rule Wrong?

↓

Yes

↓

Fix Policy Routing

↓

No

↓

Inspect Firewall
```

---

# Security Perspective

Routing internals are security critical.

Attack surfaces:

```text
ARP Poisoning

↓

Route Injection

↓

Policy Abuse

↓

Traffic Redirection
```

Protect:

```text
ARP Inspection

Route Filtering

Firewall Rules

VPN Segmentation
```

---

# Common Misconceptions

### Misconception 1

"`ip route` is the routing table."

Partially true.

Kernel FIB is the actual routing database.

---

### Misconception 2

"Applications route packets."

Wrong.

Kernel routes packets.

---

### Misconception 3

"ARP is routing."

Wrong.

ARP is neighbor discovery.

---

### Misconception 4

"Docker creates its own networking stack."

Wrong.

Docker heavily uses Linux networking.

---

### Misconception 5

"Kubernetes networking is magical."

Wrong.

It's Linux networking automation.

---

# Engineer Mental Model

Never think:

```text
Application

↓

Internet
```

Think:

```text
Application

↓

Kernel

↓

Rules

↓

FIB

↓

Neighbor Subsystem

↓

Traffic Control

↓

NIC

↓

Network
```

This is how production engineers think.

---

# WH Questions

## Why learn internals?

Because production debugging requires it.

---

## Who uses these concepts?

Linux engineers, SREs, cloud engineers, platform engineers.

---

## When do they execute?

For every packet.

---

## Where does Linux store routes?

Inside the FIB.

---

## How do tools modify networking?

Through Netlink.

---

# Key Takeaways

✅ Linux networking is many subsystems working together

✅ The kernel controls routing

✅ FIB powers forwarding decisions

✅ Netlink connects user space to the kernel

✅ Neighbor subsystem powers ARP

✅ Policy routing enables advanced networking

✅ Docker uses Linux networking

✅ Kubernetes automates Linux networking

✅ Cloud networking virtualizes Linux networking concepts

---

# Routing Section Status

Core Files:

```text
✅ routing.md

✅ routing-table.md

✅ default-gateway.md

✅ static-routing.md

✅ dynamic-routing-introduction.md

✅ packet-journey.md

✅ internals.md
```

# Recommended Next File

At this point, do **NOT** jump into OSPF/BGP yet.

The highest ROI file for capability building is:

```text
networking/Routing/troubleshooting.md
```

This file should be entirely production-focused and teach engineers:

```text
How to think when networking breaks.
```

It will likely become one of the most valuable files in the entire repository.
