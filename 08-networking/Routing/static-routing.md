# Static Routing

# Why This Matters

Up until now we have learned:

```text
routing.md

↓

routing-table.md

↓

default-gateway.md
```

Linux can already send packets.

But here's a new problem.

Suppose your infrastructure grows.

You now have:

```text
Office Network

↓

Data Center

↓

VPN Network

↓

Cloud Network

↓

Kubernetes Cluster
```

How does Linux know how to reach all of these networks?

Someone must teach Linux.

That teaching process is called:

> Static Routing

Static routes are one of the most important concepts in real infrastructure because engineers manually create them everywhere.

Used in:

- Linux servers
- Data centers
- VPNs
- Kubernetes
- Cloud environments
- Firewalls
- Hybrid cloud
- Multi-region systems

Understanding static routing is a huge milestone toward becoming an infrastructure engineer.

---

# Learning Goals

After reading this file, you should be able to answer:

- What is static routing?
- Why do we need it?
- How does Linux use static routes?
- How are static routes different from default routes?
- How do static routes work with VPNs?
- How do cloud providers use static routes?
- How does Kubernetes use static routes?
- When should engineers use static routes?
- What are the limitations of static routing?
- How do we troubleshoot static routing problems?

---

# The Big Picture

Suppose your company has two networks.

```text
Office

192.168.1.0/24

Data Center

10.10.0.0/16
```

How does your office network reach the data center?

Without instructions:

```text
Laptop

↓

???

↓

Data Center
```

Linux has no idea.

You must teach it.

---

# What Is Static Routing?

Static routing is:

> Manually telling Linux where packets should go.

You create a rule.

Linux obeys it.

Example:

```text
To reach:

10.10.0.0/16

Send packets to:

192.168.1.254
```

---

# Think Like Google Maps

Imagine this.

Google Maps doesn't know a new road exists.

You manually add:

```text
To reach Mumbai

Take Highway A
```

Static routes work exactly the same way.

You explicitly tell Linux:

```text
Destination

↓

Path
```

---

# The Core Idea

Without static route:

```text
Unknown network

↓

Default gateway
```

With static route:

```text
Unknown network

↓

Specific router
```

Specific routes always win.

---

# Visual Example

```text
Laptop

192.168.1.20

|

|

Office Router

192.168.1.254

|

|

Data Center Router

|

|

10.10.0.0/16
```

Static route:

```text
10.10.0.0/16

↓

192.168.1.254
```

---

# Why Not Use The Default Gateway?

You could.

But it may be inefficient.

Without static route:

```text
Laptop

↓

Internet Gateway

↓

Internet

↓

Data Center
```

With static route:

```text
Laptop

↓

Office Router

↓

Private Link

↓

Data Center
```

Better.

Faster.

Cheaper.

More secure.

---

# How Linux Uses Static Routes

Suppose:

```text
Destination:

10.10.20.50
```

Linux routing table:

```text
10.10.0.0/16 via 192.168.1.254

default via 192.168.1.1
```

Linux checks:

```text
Specific match exists?
```

Answer:

```text
Yes
```

Linux chooses:

```text
10.10.0.0/16
```

The default route is ignored.

---

# Static Route Selection Flow

```text
Packet Created

↓

Destination IP

↓

Check Specific Routes

↓

Found?

↓

Yes

↓

Use Static Route

↓

No

↓

Use Default Route
```

---

# Linux Command

Add route:

```bash
sudo ip route add 10.10.0.0/16 via 192.168.1.254
```

Verify:

```bash
ip route
```

Output:

```text
10.10.0.0/16 via 192.168.1.254 dev eth0
```

---

# Route Breakdown

```text
10.10.0.0/16
```

Destination network.

---

```text
via 192.168.1.254
```

Next hop router.

---

```text
dev eth0
```

Outgoing interface.

---

# Delete Route

```bash
sudo ip route del 10.10.0.0/16
```

---

# Replace Route

```bash
sudo ip route replace 10.10.0.0/16 via 192.168.1.100
```

---

# Temporary vs Persistent Routes

This is a production gotcha.

Using:

```bash
ip route add
```

is temporary.

Reboot.

Gone.

---

# Making Routes Persistent

Depends on Linux distribution.

---

## Ubuntu (Netplan)

```yaml
network:
  version: 2

  ethernets:

    eth0:

      routes:

        - to: 10.10.0.0/16

          via: 192.168.1.254
```

Apply:

```bash
sudo netplan apply
```

---

## RHEL Based Systems

NetworkManager:

```bash
nmcli connection modify eth0 +ipv4.routes "10.10.0.0/16 192.168.1.254"

nmcli connection up eth0
```

---

# Real Infrastructure Example

Company infrastructure:

```text
Office

192.168.1.0/24

↓

VPN Gateway

↓

AWS

10.0.0.0/16
```

Employees need AWS access.

Static route:

```text
10.0.0.0/16

↓

VPN Gateway
```

---

# VPN Example

Without static route:

```text
Laptop

↓

Internet

↓

AWS
```

May fail.

With route:

```text
Laptop

↓

VPN Tunnel

↓

AWS
```

Works.

---

# Split Tunneling Example

Goal:

```text
Company traffic

↓

VPN

Internet traffic

↓

Normal ISP
```

Static route:

```text
10.0.0.0/16 via VPN

10.1.0.0/16 via VPN
```

Everything else:

```text
Default Gateway
```

This is called:

> Split Tunneling

Extremely common.

---

# Docker Example

Docker automatically creates static routes.

Host routing table:

```text
172.17.0.0/16 dev docker0
```

Meaning:

```text
Container traffic

↓

docker0
```

---

# Kubernetes Example

Suppose:

```text
Node A

10.244.1.0/24

Node B

10.244.2.0/24
```

Node A:

```text
10.244.2.0/24 via 192.168.10.22
```

This is a static route.

Many CNI plugins generate these automatically.

Examples:

```text
Flannel

Calico

Canal
```

---

# Cloud Example (AWS)

AWS route table:

```text
10.0.0.0/16 local

172.31.0.0/16 vpc-peering

0.0.0.0/0 igw
```

This is essentially static routing.

AWS manages it for you.

---

# Multi VPC Example

```text
VPC A

10.0.0.0/16

↓

Peering

↓

VPC B

172.31.0.0/16
```

Route:

```text
172.31.0.0/16

↓

VPC Peering Connection
```

Without it:

Communication fails.

---

# Linux Internals

When packet arrives:

```text
Application

↓

Socket

↓

TCP/UDP

↓

IP Layer

↓

FIB Lookup

↓

Static Route Found

↓

Neighbor Lookup

↓

NIC Driver

↓

Physical Network
```

Static routes are stored inside:

```text
FIB

Forwarding Information Base
```

---

# Packet Journey Example

Goal:

```text
192.168.1.20

↓

10.10.20.50
```

Journey:

```text
Application

↓

Kernel

↓

Routing Table

↓

192.168.1.254

↓

Data Center Router

↓

Destination
```

---

# Production Scenario 1

## Symptom

VPN connected.

Cannot access company network.

Check:

```bash
ip route
```

Missing:

```text
10.0.0.0/16
```

Root cause:

```text
VPN route missing
```

---

# Production Scenario 2

## Symptom

Kubernetes Pods unreachable.

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
Broken CNI plugin
```

---

# Production Scenario 3

## Symptom

Cloud VPCs cannot communicate.

Check:

```text
VPC Route Tables
```

Missing:

```text
Peering route
```

---

# Static Routing Does NOT Scale Well

Imagine:

1000 routers.

1000 networks.

Manual routes everywhere.

```text
Server 1

Server 2

Server 3

Server 4

...

1000
```

Maintenance becomes impossible.

This problem gave birth to:

> Dynamic Routing Protocols

We'll learn those next.

---

# Static vs Dynamic Routing

| Feature | Static | Dynamic |
|---------|--------|---------|
| Manual configuration | Yes | No |
| Learns changes automatically | No | Yes |
| Scales well | No | Yes |
| Complexity | Low | High |
| Resource usage | Low | Higher |
| Suitable for small networks | Excellent | Good |
| Suitable for huge networks | Poor | Excellent |

---

# When Engineers Use Static Routing

Good use cases:

✅ VPNs

✅ Small networks

✅ Backup routes

✅ Cloud networks

✅ Kubernetes node communication

✅ Security segmentation

---

Avoid using static routing for:

❌ Thousands of routers

❌ Rapidly changing networks

❌ Internet-scale infrastructure

---

# Troubleshooting Workflow

Question 1:

What is my destination?

---

Question 2:

Do I have a route?

```bash
ip route
```

---

Question 3:

Can I reach next hop?

```bash
ping gateway-ip
```

---

Question 4:

Can I see the path?

```bash
traceroute destination
```

---

Question 5:

Does the remote network know how to return traffic?

Remember:

> Routing is bidirectional.

Very common production mistake.

---

# The Return Path Problem

This is one of the biggest beginner mistakes.

Request:

```text
Server A

↓

Server B
```

Works.

Response:

```text
Server B

↓

???

↓

Server A
```

Fails.

Every route needs a return path.

Networking is two-way communication.

---

# Troubleshooting Decision Tree

```text
Cannot Reach Network

↓

ip route

↓

Route Exists?

↓

No

↓

Add Route

↓

Yes

↓

Ping Next Hop

↓

Reachable?

↓

No

↓

Investigate Router

↓

Yes

↓

Traceroute

↓

Find Broken Hop
```

---

# Security Considerations

Bad routes can:

- Leak private traffic
- Bypass VPNs
- Expose databases
- Create unintended internet paths
- Break segmentation

Always validate:

```text
Source

↓

Destination

↓

Next Hop

↓

Return Path
```

---

# Common Misconceptions

### Misconception 1

"Default gateway is enough."

Wrong.

Many infrastructures require explicit routes.

---

### Misconception 2

"Static routing is outdated."

Wrong.

Used everywhere.

---

### Misconception 3

"Cloud providers don't use static routing."

Wrong.

Cloud networking heavily depends on it.

---

### Misconception 4

"Only routers use static routes."

Wrong.

Linux servers use them too.

---

# Engineer Mental Model

Never think:

```text
I am connecting to another server.
```

Think:

```text
Destination Network

↓

Route

↓

Next Hop

↓

Router

↓

Destination
```

This mental model scales from:

```text
Laptop

↓

Data Center

↓

Cloud

↓

Kubernetes

↓

Global Infrastructure
```

---

# WH Questions

## Why do static routes exist?

To manually teach Linux how to reach specific networks.

---

## Who creates them?

Engineers, routers, cloud systems, automation tools.

---

## When should we use them?

When networks are predictable and stable.

---

## Where are they stored?

Inside the Linux kernel FIB.

---

## How are they prioritized?

Longest prefix match.

Then metrics.

---

# Key Takeaways

✅ Static routing is manual routing

✅ Specific routes beat default routes

✅ Linux stores routes inside the FIB

✅ Static routes are everywhere

✅ Docker creates static routes

✅ Kubernetes heavily relies on routes

✅ Cloud networking uses routes extensively

✅ Return paths are mandatory

✅ Static routing eventually hits scalability limits

---

# Recommended Next File

```text
networking/Routing/dynamic-routing-introduction.md
```

# Repository Expansion Recommendation

The Routing section is becoming large enough that these additional files should eventually be created:

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

visuals.md

comparisons.md

labs.md
```

Future `labs.md` recommendation:

```text
Lab 1 - Build two Linux routers

Lab 2 - Add static routes

Lab 3 - Build VPN routing

Lab 4 - Docker networking routes

Lab 5 - Kubernetes Pod routing

Lab 6 - AWS VPC route simulation
```
