# Linux VXLAN

# Building Networks That Span Multiple Servers

---

# Why This File Exists

Up until now we have learned:

```text
Network Namespace

↓

veth

↓

Linux Bridge

↓

VLAN
```

All of these work very well.

But there is a huge problem.

---

# The Modern Infrastructure Problem

Suppose we have two servers.

```text
Server A

192.168.1.10

Server B

192.168.1.20
```

Server A hosts:

```text
Container A

Container B
```

Server B hosts:

```text
Container C

Container D
```

Question:

> How can Container A communicate with Container C as if they are on the same network?

This problem led to VXLAN.

---

# What Is VXLAN?

VXLAN = Virtual eXtensible LAN

It creates:

> Layer 2 networks over Layer 3 infrastructure.

Simply:

```text
Ethernet Network

↓

Encapsulated

↓

IP Network

↓

Delivered Anywhere
```

Think:

> Build a virtual switch that spans multiple physical machines.

---

# Mental Model

Imagine cities.

Without VXLAN:

```text
City A

Roads

Cannot directly connect

City B
```

With VXLAN:

```text
City A

↓

Underground Tunnel

↓

City B
```

VXLAN is the tunnel.

---

# Learning Goals

After this file you should understand:

* Why VXLAN exists
* VXLAN architecture
* Overlay networks
* Underlay networks
* VTEPs
* VNI
* Encapsulation
* Packet journeys
* Kubernetes networking
* Cloud networking
* Production troubleshooting

---

# Evolution Of Networking

```mermaid
flowchart TD

PHYSICAL[Physical Network]

PHYSICAL --> VLAN[VLAN]

VLAN --> LIMIT[Multi-Host Limitation]

LIMIT --> VXLAN

VXLAN --> CLOUD[Cloud Native Networking]

CLOUD --> KUBERNETES[Kubernetes]

KUBERNETES --> MODERN[Modern Infrastructure]
```

---

# The Core Problem

Bridge works inside one machine.

VLAN works inside one physical network.

Neither easily solves:

```text
Multi Host Container Networking
```

---

# Visual: Traditional Architecture

```mermaid
graph TD

subgraph SERVER_A

A[Container A]

B[Container B]

end

subgraph SERVER_B

C[Container C]

D[Container D]

end

SERVER_A --> NETWORK[Physical Network]

SERVER_B --> NETWORK
```

Containers are isolated per host.

---

# Enter Overlay Networking

VXLAN introduces overlay networking.

---

# Underlay vs Overlay

## Underlay

The real physical network.

```mermaid
graph LR

R1[Router]

R2[Router]

R3[Router]

R1 --- R2

R2 --- R3
```

---

## Overlay

The virtual network.

```mermaid
graph LR

C1[Container]

C2[Container]

C3[Container]

C1 --- C2

C2 --- C3
```

Applications only see overlay.

---

# Visual Architecture

```mermaid
graph TD

subgraph Overlay Network

A[Container A]

C[Container C]

end

subgraph Underlay Network

S1[Server A]

S2[Server B]

R[Physical Network]

end

A --> S1

S1 --> R

R --> S2

S2 --> C
```

---

# Core VXLAN Components

There are 4 major components.

```mermaid
mindmap

root((VXLAN))

VTEP

VNI

Overlay

Encapsulation
```

---

# Component 1: VTEP

VTEP = VXLAN Tunnel Endpoint.

Think:

> Tunnel entrance and exit.

Every host participating in VXLAN becomes a VTEP.

---

# VTEP Architecture

```mermaid
graph LR

A[Container A]

V1[VTEP A]

NET[IP Network]

V2[VTEP B]

B[Container B]

A --> V1

V1 --> NET

NET --> V2

V2 --> B
```

---

# Component 2: VNI

VNI = VXLAN Network Identifier.

Think:

```text
VLAN → Apartment Number

VNI → Entire Building Number
```

---

# VLAN vs VXLAN Scale

| Technology | Networks Supported |
| ---------- | ------------------ |
| VLAN       | 4094               |
| VXLAN      | 16 million         |

---

# Why VXLAN Needed Expansion

4094 VLANs are not enough for:

* AWS
* Azure
* GCP
* Kubernetes
* Multi-tenant platforms

VXLAN solved this.

---

# VNI Visual

```mermaid
graph TD

VNI1[VNI 100]

VNI2[VNI 200]

VNI3[VNI 300]

VNI4[VNI 400]
```

Each VNI is an independent network.

---

# Encapsulation

This is the most important concept.

Original packet:

```text
Ethernet

IP

TCP

Data
```

VXLAN wraps it.

---

# VXLAN Packet Structure

```mermaid
flowchart LR

OUTER_ETH[Outer Ethernet]

OUTER_IP[Outer IP]

UDP[UDP 4789]

VXLAN[VXLAN Header]

INNER_ETH[Inner Ethernet]

INNER_IP[Inner IP]

TCP[TCP]

DATA[Data]

OUTER_ETH --> OUTER_IP

OUTER_IP --> UDP

UDP --> VXLAN

VXLAN --> INNER_ETH

INNER_ETH --> INNER_IP

INNER_IP --> TCP

TCP --> DATA
```

---

# Why UDP?

Because routers understand IP and UDP.

The physical network simply forwards packets.

It doesn't know about containers.

---

# End-to-End Packet Journey

Container A:

```text
10.10.1.2
```

Container C:

```text
10.10.1.3
```

Journey:

```mermaid
sequenceDiagram

participant CA as Container A

participant VA as VTEP A

participant NET as Physical Network

participant VB as VTEP B

participant CB as Container B

CA->>VA: Original Ethernet Frame

VA->>NET: Encapsulate VXLAN

NET->>VB: Deliver UDP Packet

VB->>CB: Decapsulate Frame
```

---

# Visual: Full Architecture

```mermaid
graph TD

subgraph SERVER_A

A[Container A]

B[Container B]

VTEPA[VTEP]

end

subgraph SERVER_B

C[Container C]

D[Container D]

VTEPB[VTEP]

end

A --> VTEPA

B --> VTEPA

VTEPA --> CLOUD[IP Network]

CLOUD --> VTEPB

VTEPB --> C

VTEPB --> D
```

---

# Linux VXLAN Interface

Linux supports VXLAN natively.

Create interface:

```bash
sudo ip link add vxlan100 type vxlan \
id 100 \
dev eth0 \
remote 192.168.1.20 \
dstport 4789
```

Bring up:

```bash
sudo ip link set vxlan100 up
```

Verify:

```bash
ip link
```

---

# Docker Overlay Networking

Docker Swarm uses VXLAN.

Architecture:

```mermaid
graph TD

ContainerA --> VXLAN

ContainerB --> VXLAN

ContainerC --> VXLAN

VXLAN --> MultiHost[Multi Host Network]
```

---

# Kubernetes Uses VXLAN

Many CNI plugins use VXLAN.

Examples:

```text
Flannel

Calico (optional)

Canal

OpenShift SDN
```

---

# Kubernetes Architecture

```mermaid
graph TD

subgraph Node1

P1[Pod A]

V1[VTEP]

end

subgraph Node2

P2[Pod B]

V2[VTEP]

end

P1 --> V1

V1 --> NET[Cluster Network]

NET --> V2

V2 --> P2
```

---

# Cloud Networking Relationship

Cloud providers abstract this away.

Internally:

```text
Tenant Isolation

↓

Overlay Networks

↓

Encapsulation

↓

Distributed Fabrics
```

The ideas are similar.

---

# Production Multi-Tenant Architecture

```mermaid
graph TD

TENANT1[Tenant A]

TENANT2[Tenant B]

TENANT3[Tenant C]

TENANT1 --> VNI100[VNI100]

TENANT2 --> VNI200[VNI200]

TENANT3 --> VNI300[VNI300]

VNI100 --> FABRIC[Cloud Fabric]

VNI200 --> FABRIC

VNI300 --> FABRIC
```

---

# Performance Tradeoff

VXLAN adds overhead.

Extra headers mean:

```text
Original Packet

+

Outer Ethernet

+

Outer IP

+

UDP

+

VXLAN Header
```

More bytes.

---

# MTU Problem

One of the most common production issues.

Example:

```text
Physical MTU = 1500

VXLAN Overhead ≈ 50 bytes
```

Recommended:

```text
1450
```

or

```text
9000 Jumbo Frames
```

depending on infrastructure.

---

# Troubleshooting Decision Tree

```mermaid
flowchart TD

START[Pod Cannot Reach Another Pod]

START --> VTEP[VTEP Exists?]

VTEP -->|No| FIX1[Configure VTEP]

VTEP -->|Yes| UDP[UDP 4789 Allowed?]

UDP -->|No| FIX2[Open Firewall]

UDP -->|Yes| VNI[VNI Match?]

VNI -->|No| FIX3[Correct VNI]

VNI -->|Yes| MTU[Check MTU]

MTU --> SUCCESS[Working]
```

---

# Production Problems

## Problem 1

Pods communicate on same node.

Fail across nodes.

Possible causes:

```text
UDP 4789 blocked
```

---

## Problem 2

Intermittent packet loss.

Possible causes:

```text
MTU mismatch
```

---

## Problem 3

Cluster instability.

Possible causes:

```text
Wrong VNI
```

---

## Problem 4

High latency.

Possible causes:

```text
Excessive encapsulation

CPU overhead
```

---

# Essential Commands

Show interfaces:

```bash
ip link
```

Show routes:

```bash
ip route
```

Show VXLAN:

```bash
bridge fdb show
```

Show sockets:

```bash
ss -lunp
```

Capture packets:

```bash
sudo tcpdump -i eth0 udp port 4789
```

Show neighbor table:

```bash
ip neigh
```

---

# Common Misconceptions

### Misconception 1

> VXLAN replaces VLAN.

Wrong.

VXLAN extends VLAN concepts.

---

### Misconception 2

> VXLAN is Kubernetes.

Wrong.

Kubernetes can use VXLAN.

---

### Misconception 3

> Overlay networking replaces physical networking.

Wrong.

Overlay depends on underlay.

---

# Engineer Mental Model

Never think:

```text
Pod A

↓

Pod B
```

Always think:

```mermaid
flowchart TD

PODA[Pod A]

PODA --> VTEPA[VTEP A]

VTEPA --> UNDERLAY[Physical Network]

UNDERLAY --> VTEPB[VTEP B]

VTEPB --> PODB[Pod B]
```

---

# Capability Checklist

After this file you should understand:

✅ Overlay networking

✅ Underlay networking

✅ VTEP

✅ VNI

✅ Encapsulation

✅ Multi-host networking

✅ Kubernetes networking foundations

✅ Production troubleshooting

