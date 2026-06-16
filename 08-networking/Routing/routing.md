# Routing

# Why This Matters

Every application you build eventually asks a very simple question:

> "How do I send this packet to another machine?"

Routing is Linux answering that question.

Whether it's:

- Opening google.com
- Calling a microservice
- Pulling a Docker image
- Connecting to a database
- Kubernetes Pod communication
- VPN traffic
- Cloud networking
- Cross-region communication

Everything eventually becomes:

> Where should this packet go next?

Routing is one of the most fundamental concepts in networking because **every packet on every machine is routed.**

---

# Learning Goals

After reading this file, you should be able to answer:

- What is routing?
- Why do we need routing?
- How does Linux decide where packets go?
- What is a route?
- What is a next hop?
- What is a gateway?
- What is a routing table?
- What happens if no route exists?
- How do routers work?
- How do cloud networks route traffic?
- How does Kubernetes route Pod traffic?
- How do engineers troubleshoot routing problems?

---

# The Big Picture

Imagine sending a physical package.

You have:

- Destination address
- Roads
- Intersections
- Highway exits
- Delivery trucks

Network routing is exactly the same.

Packet delivery requires decisions.

```text
Source Machine
      |
      |
      v
Router A
      |
      |
      v
Router B
      |
      |
      v
Router C
      |
      |
      v
Destination
```

Every hop asks:

```text
Where should I send this packet next?
```

That decision is routing.

---

# What Is Routing?

Routing is:

> The process of selecting the path a packet should take to reach its destination.

Linux performs routing for every outgoing packet.

Routers perform routing for every forwarded packet.

Cloud providers perform routing for every virtual packet.

Kubernetes performs routing for Pod communication.

---

# Core Terminology

## Packet

Small unit of network data.

Contains:

```text
+------------------------+
| Source IP              |
| Destination IP         |
| Protocol               |
| Payload                |
+------------------------+
```

---

## Route

A rule that says:

```text
To reach this network,
send packets this way.
```

Example:

```text
Destination: 192.168.1.0/24

Use interface: eth0
```

---

## Next Hop

The immediate device that receives the packet.

Not necessarily the final destination.

Example:

```text
Laptop
 |
 |
Router
 |
 |
Internet
 |
 |
Google
```

Laptop sends packet to:

```text
Router
```

Router is the next hop.

---

## Router

A device that forwards packets between networks.

Examples:

Physical router:

```text
Home Router
```

Cloud router:

```text
AWS VPC Router
```

Software router:

```text
Linux Server
```

---

# Routing Exists Everywhere

## Your Laptop

```text
Laptop
 |
 | WiFi
 |
Router
 |
Internet
```

---

## Data Center

```text
Application Server
         |
         |
Top Of Rack Switch
         |
Aggregation Router
         |
Core Router
         |
Internet
```

---

## Kubernetes

```text
Pod A
 |
Node A
 |
Cluster Network
 |
Node B
 |
Pod B
```

---

## Cloud

```text
EC2
 |
VPC Router
 |
Internet Gateway
 |
Internet
```

---

# Linux Is Also A Router

Many beginners think:

> Routers are special hardware.

Not true.

Linux itself can become a router.

Enable IP forwarding:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Now Linux can forward packets between interfaces.

Example:

```text
eth0 ---> Linux Server ---> eth1
```

This is extremely common.

Used in:

- VPN servers
- Kubernetes nodes
- NAT gateways
- Firewalls
- Service meshes

---

# The Linux Routing Decision Process

Whenever an application sends data:

```text
Application
      |
Socket
      |
TCP/UDP
      |
IP Layer
      |
Routing Lookup
      |
Network Interface
      |
NIC
      |
Wire
```

The kernel performs routing lookup.

This is one of Linux's most important jobs.

---

# Routing Decision Flow

```text
Packet Created
       |
       v
Extract Destination IP
       |
       v
Check Routing Table
       |
       |
Route Found?
   /       \
 Yes        No
 |          |
 v          v
Forward    Drop Packet
 |
 v
Send Through Interface
```

---

# Example

Suppose your machine is:

```text
IP: 192.168.1.20
```

You access:

```text
8.8.8.8
```

Linux checks:

```text
How do I reach 8.8.8.8?
```

Routing table:

```text
192.168.1.0/24 dev eth0

default via 192.168.1.1 dev eth0
```

Linux sees:

```text
8.8.8.8
```

does not belong to:

```text
192.168.1.0/24
```

So:

```text
Use default route
```

Packet goes to:

```text
192.168.1.1
```

which is your router.

---

# Longest Prefix Match

Linux chooses the most specific route.

This is called:

> Longest Prefix Match

Example:

Routing table:

```text
10.0.0.0/8

10.1.0.0/16

10.1.1.0/24
```

Destination:

```text
10.1.1.50
```

Linux chooses:

```text
10.1.1.0/24
```

because it is the most specific.

---

# Visual Example

```text
10.0.0.0/8
|
|-----10.1.0.0/16
        |
        |------10.1.1.0/24
```

Specificity wins.

This principle powers the entire internet.

---

# How A Packet Travels

Suppose:

```text
Laptop: 192.168.1.20

Google DNS: 8.8.8.8
```

Journey:

```text
Laptop
 |
 | Packet
 |
Home Router
 |
ISP Router
 |
ISP Core Router
 |
Transit Network
 |
Google Edge Router
 |
Google Server
```

Every device performs routing decisions.

---

# Packet Journey Visualization

```text
Application

↓

Socket

↓

TCP

↓

IP Layer

↓

Routing Lookup

↓

Gateway Selection

↓

ARP Resolution

↓

NIC

↓

Switch

↓

Router

↓

Internet
```

Routing is only one step.

Many other systems participate.

---

# Local Network vs Remote Network

Local network:

```text
192.168.1.0/24
```

Devices communicate directly.

```text
Laptop ---> Printer
```

No router needed.

Remote network:

```text
Laptop ---> Router ---> Internet
```

Router required.

---

# How Linux Knows It's Local

Suppose:

```text
Machine:

192.168.1.20/24
```

CIDR:

```text
/24
```

means:

```text
192.168.1.0 - 192.168.1.255
```

Anything inside:

```text
192.168.1.x
```

is local.

Everything else:

```text
Send to router.
```

---

# Real Linux Commands

See routes:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev wlan0

192.168.1.0/24 dev wlan0 proto kernel scope link src 192.168.1.20
```

---

See interfaces:

```bash
ip addr
```

See neighbors:

```bash
ip neigh
```

See packet statistics:

```bash
ip -s route
```

---

# Modern Infrastructure Examples

## Docker

Docker creates routes.

```text
Container

172.17.0.2

↓

docker0 bridge

↓

Host

↓

Internet
```

Check:

```bash
ip route
```

You may see:

```text
172.17.0.0/16 dev docker0
```

---

# Kubernetes

Pod:

```text
10.244.1.10
```

Node routing table:

```text
10.244.2.0/24 via 192.168.10.22
```

Meaning:

```text
To reach Node B Pods,
send traffic to Node B.
```

Kubernetes networking is heavily dependent on routing.

---

# Cloud Example (AWS)

EC2:

```text
10.0.1.25
```

VPC Route Table:

```text
10.0.0.0/16 local

0.0.0.0/0 igw
```

Meaning:

```text
Local traffic stays inside VPC

Internet traffic goes to Internet Gateway
```

Routing exists even though you don't see physical routers.

---

# Production Scenario 1

### Symptom

```text
Server cannot reach internet.
```

Check:

```bash
ip route
```

Output:

```text
192.168.1.0/24 dev eth0
```

Missing:

```text
default route
```

Root cause:

```text
No path to internet.
```

Fix:

```bash
sudo ip route add default via 192.168.1.1
```

---

# Production Scenario 2

### Symptom

Pods cannot communicate.

Investigate:

```bash
ip route
```

Missing routes:

```text
10.244.x.x
```

Potential issue:

```text
CNI plugin failure
```

Common in:

- Flannel
- Calico
- Weave

---

# Production Scenario 3

### Symptom

VPN connected but internal resources unreachable.

Check:

```bash
ip route
```

Possible issue:

```text
VPN routes not injected.
```

---

# Troubleshooting Mental Model

Always ask:

### Question 1

Where am I?

```bash
ip addr
```

---

### Question 2

Where am I trying to go?

Destination IP?

---

### Question 3

Do I have a route?

```bash
ip route
```

---

### Question 4

Who is my next hop?

```text
gateway
```

---

### Question 5

Can I reach the next hop?

```bash
ping gateway-ip
```

---

### Question 6

Can I see the packet path?

```bash
traceroute destination
```

or

```bash
tracepath destination
```

---

# Engineer Mental Model

Never think:

> Server talks to another server.

Think:

```text
Application

↓

Socket

↓

Kernel

↓

Routing

↓

Gateway

↓

Switch

↓

Router

↓

Network

↓

Destination
```

This mental model solves countless production incidents.

---

# Security Considerations

Routing mistakes can expose systems.

Examples:

Bad route:

```text
0.0.0.0/0
```

through:

```text
Public interface
```

Could expose:

- Internal databases
- Admin interfaces
- Kubernetes APIs

Always verify:

- Route tables
- Firewalls
- Security groups
- Network ACLs

Routing and security are tightly coupled.

---

# Common Misconceptions

### Misconception 1

"DNS is routing."

Wrong.

DNS:

```text
Name -> IP
```

Routing:

```text
IP -> Path
```

---

### Misconception 2

"Switches and routers are the same."

Wrong.

Switch:

```text
MAC addresses
```

Router:

```text
IP addresses
```

---

### Misconception 3

"Internet is one network."

Wrong.

Internet is:

```text
Thousands of interconnected networks.
```

---

### Misconception 4

"Only routers perform routing."

Wrong.

Linux performs routing too.

---

# Routing In One Sentence

> Routing is the decision-making process that determines where every packet should go next.

---

# Key Takeaways

✅ Every packet is routed

✅ Linux performs routing

✅ Routers forward packets between networks

✅ Routing tables contain forwarding decisions

✅ Longest prefix match selects routes

✅ Default routes send unknown traffic

✅ Kubernetes heavily relies on routing

✅ Cloud networking is routing at scale

✅ Troubleshooting routing is a core engineer skill

✅ Understanding routing is foundational for backend, DevOps, cloud, and SRE engineering

---

# Recommended Supporting Files

This topic should eventually expand into:

```text
networking/Routing/

routing.md

routing-table.md

static-routing.md

dynamic-routing-introduction.md

default-gateway.md

internals.md

packet-journey.md

troubleshooting.md

security.md

comparisons.md

visuals.md
```

Recommended learning order:

```text
routing.md
        ↓
routing-table.md
        ↓
default-gateway.md
        ↓
static-routing.md
        ↓
dynamic-routing-introduction.md
        ↓
packet-journey.md
        ↓
internals.md
        ↓
troubleshooting.md
```
