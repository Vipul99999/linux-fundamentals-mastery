# Linux Kernel Networking

# Understanding How The Linux Kernel Moves Packets

---

# Why this file exists

Most engineers learn:

```text
Application

↓

TCP

↓

IP

↓

Internet
```

But Linux networking is much more than that.

The Linux kernel is a massive packet processing engine.

Understanding this file helps you understand:

* Docker networking
* Kubernetes networking
* Firewalls
* Service meshes
* CNI plugins
* Cloud networking
* High-performance systems
* DDoS mitigation
* Production troubleshooting

This is one of the most important Linux engineering topics.

---

# Mental Model

Think of Linux kernel networking as a giant factory.

Packets are products moving through conveyor belts.

At every station Linux can:

```text
Inspect

Modify

Drop

Encrypt

Forward

Queue

Route

Prioritize
```

---

# Big Picture

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

Routing Subsystem

↓

Netfilter

↓

Traffic Control

↓

Device Driver

↓

NIC

↓

Wire
```

Everything after the application runs inside kernel space.

---

# User Space vs Kernel Space

Applications do not directly access hardware.

```text
+----------------------+

USER SPACE

nginx

nodejs

redis

postgres

docker

kubelet

+----------------------+

|

System Calls

|

+----------------------+

KERNEL SPACE

Networking Stack

Memory Management

Scheduler

Filesystem

Drivers

+----------------------+
```

The kernel controls everything.

---

# The Linux Networking Subsystems

Linux networking is composed of many subsystems.

```text
Socket Layer

TCP

UDP

IP

ARP

Routing

Netfilter

Traffic Control

Device Drivers

NIC subsystem
```

These all cooperate together.

---

# Linux Networking Architecture

```text
                 User Space

+---------------------------------+

Application

+---------------------------------+

              |

              | System Call

              |

              v

                 Kernel Space


+---------------------------------+

Socket Layer

+---------------------------------+

TCP / UDP

+---------------------------------+

IP Layer

+---------------------------------+

Routing Subsystem

+---------------------------------+

Netfilter

+---------------------------------+

Traffic Control

+---------------------------------+

Device Driver

+---------------------------------+

NIC Hardware

+---------------------------------+
```

---

# What Happens During socket()

Suppose nginx starts.

```c
socket(AF_INET, SOCK_STREAM,0);
```

Linux creates:

```text
Socket Object

↓

Kernel Data Structures

↓

Buffer Allocation

↓

File Descriptor

↓

Connection Tracking
```

Kernel returns:

```text
fd=3
```

Everything uses file descriptors.

Linux philosophy:

```text
Everything is a file
```

Even sockets.

---

# Important Kernel Data Structures

These are worth remembering.

---

# socket

Represents communication endpoint.

Contains:

```text
Protocol

Buffers

State

Callbacks

Flags
```

---

# sock

Protocol-specific implementation.

Contains:

```text
TCP state

Sequence numbers

Ports

Timers
```

---

# sk_buff (skb)

This is extremely important.

`sk_buff`

is the fundamental packet object.

Linux stores every packet inside skb structures.

Think:

```text
Packet

↓

sk_buff

↓

Linux moves skb everywhere
```

---

# skb Visualization

```text
+------------------------+

sk_buff

+------------------------+

Metadata

Pointers

Headers

Payload

Timestamp

Priority

Flags

+------------------------+
```

The packet itself is not moved.

Linux often moves pointers.

This improves performance.

---

# The Life Of A Packet

```text
NIC Receives Packet

↓

Driver Creates skb

↓

Kernel Processes skb

↓

Firewall Checks skb

↓

Routing Checks skb

↓

TCP Processes skb

↓

Socket Receives skb

↓

Application Receives Data
```

Everything revolves around skb.

---

# Kernel Receive Path (RX)

Incoming traffic.

```text
Internet

↓

NIC

↓

Driver

↓

sk_buff

↓

Netfilter

↓

Routing

↓

TCP

↓

Socket

↓

Application
```

---

# Kernel Transmit Path (TX)

Outgoing traffic.

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Routing

↓

Netfilter

↓

Traffic Control

↓

Driver

↓

NIC

↓

Internet
```

---

# SoftIRQ

Imagine every packet generating an interrupt.

100 million packets.

CPU dies.

Linux solves this.

Linux uses:

```text
Interrupt

↓

SoftIRQ

↓

Packet Processing
```

SoftIRQ defers heavy work.

---

# SoftIRQ Visualization

```text
Packet arrives

↓

Hardware Interrupt

↓

Very small work

↓

Wake SoftIRQ

↓

Continue packet processing
```

This prevents CPU overload.

---

# NAPI (New API)

Modern Linux NIC optimization.

Old approach:

```text
Packet

↓

Interrupt

↓

CPU

↓

Packet

↓

Interrupt

↓

CPU

↓

Packet

↓

Interrupt

↓

CPU
```

CPU explodes under load.

---

# NAPI Solution

Linux switches modes.

Low traffic:

```text
Interrupt mode
```

High traffic:

```text
Polling mode
```

Visualization:

```text
Traffic Low

↓

Interrupt

↓

Traffic High

↓

Polling
```

Much faster.

---

# Receive Queue

NIC doesn't send packets one by one.

Packets enter queues.

```text
NIC

↓

RX Queue

↓

CPU
```

Modern servers:

```text
RX Queue 0 → CPU0

RX Queue 1 → CPU1

RX Queue 2 → CPU2

RX Queue 3 → CPU3
```

This improves parallelism.

---

# Transmit Queue

Outgoing packets.

```text
CPU

↓

TX Queue

↓

NIC

↓

Internet
```

---

# Multi Queue Networking

Modern servers have many queues.

Example:

```text
CPU0 → Queue0

CPU1 → Queue1

CPU2 → Queue2

CPU3 → Queue3
```

This is called:

```text
Multiqueue networking
```

Essential for:

* Cloud
* Kubernetes
* High traffic systems

---

# RSS (Receive Side Scaling)

Distributes packets.

Without RSS:

```text
100000 packets

↓

CPU0
```

Bad.

With RSS:

```text
100000 packets

↓

CPU0

CPU1

CPU2

CPU3
```

Balanced load.

---

# RPS (Receive Packet Steering)

Kernel-level balancing.

Moves packet processing across CPUs.

```text
NIC

↓

CPU0

↓

Kernel redistributes

↓

CPU1

CPU2

CPU3
```

---

# RFS (Receive Flow Steering)

Even smarter.

Linux sends flows to CPUs running the application.

Example:

```text
nginx running on CPU4

↓

Flow directed to CPU4
```

Better cache usage.

Lower latency.

---

# Traffic Control Subsystem

Kernel traffic manager.

Commands:

```bash
tc
```

Responsibilities:

```text
Delay

Limit

Shape

Prioritize

Drop
```

---

# Queue Disciplines (Qdisc)

Think:

```text
Traffic manager
```

Examples:

```text
fq_codel

fq

htb

pfifo_fast

cake
```

---

# fq_codel

Modern default.

Solves:

```text
Bufferbloat
```

Without it:

```text
Large queues

↓

Latency explodes
```

With fq_codel:

```text
Small queues

↓

Stable latency
```

---

# Netfilter Internals

Firewall engine.

Hooks:

```text
PREROUTING

INPUT

FORWARD

OUTPUT

POSTROUTING
```

Packet interception points.

---

# Connection Tracking (conntrack)

Kernel remembers connections.

Example:

```text
TCP Handshake

↓

Connection created

↓

Kernel tracks state

↓

ESTABLISHED
```

States:

```text
NEW

ESTABLISHED

RELATED

INVALID
```

Very important.

Docker and Kubernetes depend on this.

---

# Neighbor Subsystem

Linux maintains neighbor cache.

Equivalent:

```text
IP

↓

MAC Address
```

Command:

```bash
ip neigh
```

Example:

```text
192.168.1.1

↓

00:50:56:ab:cd:ef
```

---

# ARP Workflow

```text
Need destination MAC

↓

ARP Request

↓

ARP Reply

↓

Store in cache

↓

Send packet
```

---

# Modern Virtual Networking

Linux kernel powers:

```text
Docker

Kubernetes

OpenShift

LXC

Podman

VMware

KVM

Cloud Hypervisors
```

Nothing magical.

All use Linux primitives.

---

# Cloud Networking Reality

AWS:

```text
Application

↓

Linux Kernel

↓

ENA Driver

↓

Nitro Hypervisor

↓

AWS Fabric
```

Azure:

```text
Application

↓

Linux Kernel

↓

Virtual NIC

↓

Azure Fabric
```

GCP:

```text
Application

↓

Linux Kernel

↓

gVNIC

↓

Google Fabric
```

Cloud is Linux networking at massive scale.

---

# Production Bottlenecks

Engineers often discover bottlenecks here.

```text
Socket Buffers

RX Queue

TX Queue

Conntrack

Firewall Rules

CPU Interrupts

NIC Saturation
```

---

# Production Debugging Flow

```text
High latency

↓

CPU overloaded?

↓

Interrupt storm?

↓

Queue saturation?

↓

Firewall issue?

↓

Conntrack full?

↓

NIC overloaded?

↓

Driver issue?
```

---

# Essential Commands

### Interface information

```bash
ip link
```

---

### Driver information

```bash
ethtool -i eth0
```

---

### Queue information

```bash
ethtool -l eth0
```

---

### Statistics

```bash
ip -s link
```

---

### Interrupts

```bash
cat /proc/interrupts
```

---

### SoftIRQ

```bash
cat /proc/softirqs
```

---

### Socket statistics

```bash
ss -s
```

---

### Conntrack

```bash
conntrack -L
```

---

### Traffic Control

```bash
tc qdisc show
```

---

# Production Example 1

## Kubernetes Node Latency

Problem:

```text
Pods become slow
```

Root cause:

```text
CPU0 overloaded

↓

Interrupt storm

↓

No RSS enabled
```

Solution:

```text
Enable RSS

Distribute queues
```

---

# Production Example 2

## Docker Host Slow

Problem:

```text
Containers timeout
```

Root cause:

```text
Conntrack table full
```

Symptoms:

```text
Intermittent packet loss
```

---

# Production Example 3

## API Server High Latency

Problem:

```text
Response time spikes
```

Root cause:

```text
Bufferbloat
```

Fix:

```text
fq_codel
```

---

# Common Misconceptions

## Misconception 1

> TCP lives inside applications

Wrong.

TCP lives inside the kernel.

---

## Misconception 2

> Firewalls are separate software

Wrong.

Firewalls are kernel packet hooks.

---

## Misconception 3

> Docker networking is Docker code

Wrong.

Docker uses Linux networking.

---

## Misconception 4

> Cloud networking is completely different

Wrong.

Cloud extends Linux networking.

---

# Engineer Mental Model

Never think:

```text
App

↓

Internet
```

Think:

```text
App

↓

Socket

↓

sk_buff

↓

TCP

↓

IP

↓

Routing

↓

Netfilter

↓

Qdisc

↓

Driver

↓

NIC

↓

Internet
```

---

# Capability Checklist

After this file you should understand:

✅ Kernel space networking

✅ sk_buff

✅ RX path

✅ TX path

✅ SoftIRQ

✅ NAPI

✅ RSS

✅ RPS

✅ RFS

✅ Multi Queue networking

✅ Conntrack

✅ Traffic Control internals

✅ Modern cloud networking

✅ Production bottlenecks

---

# Next File

```text
network-namespaces.md
```

Then Docker and Kubernetes networking will start making complete sense.
