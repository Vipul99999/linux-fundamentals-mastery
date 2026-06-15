# ARP Troubleshooting

> Learn how Linux engineers diagnose local network failures using ARP, neighbor tables, and systematic troubleshooting workflows.

---

# Why This File Exists

Many networking problems are actually ARP problems.

Symptoms may look unrelated.

Examples:

```text
Cannot access printer

↓

Cannot access gateway

↓

Intermittent internet

↓

Random disconnects

↓

Only one machine affected

↓

Packet loss
```

ARP is often involved.

---

# Golden Rule ⭐⭐⭐⭐⭐

Before troubleshooting ARP, always ask:

```text
Can Layer 2 devices find each other?
```

If the answer is:

```text
NO
```

ARP becomes suspicious.

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
Internet broken
```

Think:

```text
Application

↓

TCP

↓

IP

↓

Routing

↓

ARP

↓

MAC

↓

Ethernet

↓

NIC

↓

Wire
```

Find where the chain breaks.

---

# The ARP Troubleshooting Workflow ⭐⭐⭐⭐⭐

Always follow this order.

```text
Problem

↓

Interface Up?

↓

IP Assigned?

↓

Same Subnet?

↓

Gateway Reachable?

↓

ARP Cache Healthy?

↓

MAC Learned?

↓

Traffic Flowing?
```

---

# Scenario 1: Cannot Reach Printer ⭐⭐⭐⭐⭐

Problem:

```text
Laptop

↓

Printer

↓

No response
```

---

# Visual

```text
Laptop

192.168.1.10

↓

Switch

↓

Printer

192.168.1.20
```

---

# Step 1

Check IP.

```bash
ip addr
```

Question:

```text
Do both devices have addresses?
```

---

# Step 2

Check subnet.

Question:

```text
Same subnet?
```

Example:

Good:

```text
Laptop

192.168.1.10/24

Printer

192.168.1.20/24
```

---

Bad:

```text
Laptop

192.168.1.10/24

Printer

192.168.2.20/24
```

---

# Step 3

Ping.

```bash
ping 192.168.1.20
```

---

# Step 4

Check neighbor table.

```bash
ip neigh
```

Expected:

```text
192.168.1.20

lladdr

AA:BB:CC:DD:EE:FF

REACHABLE
```

---

# Scenario 2: Cannot Access Gateway ⭐⭐⭐⭐⭐

Problem:

```text
Laptop

↓

Internet unavailable
```

---

# Visual

```text
Laptop

↓

Router

↓

Internet
```

---

# Step 1

Check default route.

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
```

---

# Step 2

Ping gateway.

```bash
ping 192.168.1.1
```

---

# Step 3

Check neighbor table.

```bash
ip neigh
```

Look for:

```text
192.168.1.1
```

---

# Scenario 3: Neighbor State Is FAILED ⭐⭐⭐⭐⭐

Suppose:

```bash
ip neigh
```

shows:

```text
192.168.1.1

FAILED
```

Linux cannot reach the device.

Possible causes:

```text
Cable unplugged

WiFi disconnected

Device offline

Wrong VLAN

Switch issue
```

---

# Neighbor State Lifecycle ⭐⭐⭐⭐⭐

Linux continuously tracks neighbors.

Visualization:

```text
REACHABLE

↓

STALE

↓

DELAY

↓

PROBE

↓

FAILED
```

---

# State Explanations

---

# REACHABLE

```text
Working normally
```

---

# STALE

```text
Old information

Still usable
```

---

# DELAY

```text
Linux waiting
```

---

# PROBE

```text
Linux verifying
```

---

# FAILED

```text
Communication failed
```

---

# Scenario 4: Duplicate IP ⭐⭐⭐⭐⭐

Two devices use:

```text
192.168.1.20
```

Symptoms:

```text
Random disconnects

Intermittent failures

Devices disappear

Flapping ARP entries
```

---

# Visualization

Bad:

```text
Laptop

↓

Printer

192.168.1.20

↓

Camera

192.168.1.20
```

Chaos.

---

# Scenario 5: Wrong Gateway ⭐⭐⭐⭐⭐

Problem:

```text
Local devices work

Internet fails
```

---

Visual:

```text
Laptop

↓

Wrong Router

↓

Dead End
```

---

# Check

```bash
ip route
```

---

# Scenario 6: ARP Cache Corruption ⭐⭐⭐⭐⭐

Linux cache contains outdated information.

Symptoms:

```text
Intermittent failures

Works after reboot

Works temporarily
```

---

# Fix

Flush cache.

```bash
sudo ip neigh flush all
```

---

# Scenario 7: ARP Spoofing ⭐⭐⭐⭐⭐

Symptoms:

```text
High latency

Unexpected disconnects

Certificate warnings

Traffic instability
```

---

# Check Gateway MAC

Observe:

```bash
ip neigh
```

Question:

```text
Does gateway MAC keep changing?
```

Suspicious.

---

# Linux Diagnostic Flow ⭐⭐⭐⭐⭐

Always use this order.

```text
1. Interface

↓

2. IP

↓

3. Route

↓

4. ARP

↓

5. Gateway

↓

6. Connectivity
```

---

# Linux Commands Cheat Sheet ⭐⭐⭐⭐⭐

---

# Show interfaces

```bash
ip link
```

---

# Show IP addresses

```bash
ip addr
```

---

# Show routes

```bash
ip route
```

---

# Show neighbors

```bash
ip neigh
```

---

# Monitor changes live

```bash
ip monitor neigh
```

---

# Ping gateway

```bash
ping 192.168.1.1
```

---

# Flush neighbor table

```bash
sudo ip neigh flush all
```

---

# Internal Linux Troubleshooting Flow ⭐⭐⭐⭐⭐

Suppose:

```text
Cannot reach website
```

Linux internally:

```text
Application

↓

DNS

↓

Socket

↓

TCP

↓

IP

↓

Routing

↓

ARP

↓

Neighbor Table

↓

NIC

↓

Internet
```

Find where it breaks.

---

# Visual: Same Network vs Different Network ⭐⭐⭐⭐⭐

Same subnet:

```text
Laptop

↓

Switch

↓

Printer
```

Different subnet:

```text
Laptop

↓

Router

↓

Internet
```

This distinction is critical.

---

# Real Production Scenario 1 ⭐⭐⭐⭐⭐

User says:

```text
Internet not working
```

Engineer checks:

```bash
ip addr
```

Good.

---

Checks:

```bash
ip route
```

Good.

---

Checks:

```bash
ip neigh
```

Gateway:

```text
FAILED
```

Problem found.

---

# Real Production Scenario 2 ⭐⭐⭐⭐⭐

Users report:

```text
Intermittent failures
```

Engineer observes:

```text
Gateway MAC changing repeatedly
```

Suspicious.

Investigate further.

---

# Real Production Scenario 3 ⭐⭐⭐⭐⭐

Containers cannot communicate.

Check:

```text
Docker bridge

↓

Neighbor entries

↓

Bridge configuration
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

```text
Host

↓

docker0

↓

Container A

↓

Container B
```

ARP still exists.

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

```text
Pod

↓

veth

↓

Bridge

↓

Node
```

ARP still exists underneath.

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud providers abstract ARP.

But engineers still troubleshoot:

```text
Virtual Interfaces

↓

Overlay Networks

↓

Neighbor Discovery
```

---

# Common Misconceptions

❌ Cannot access internet = DNS problem

Wrong.

ARP may be broken.

---

❌ ARP only matters for printers

Wrong.

ARP is everywhere.

---

❌ Reboot fixes networking

Wrong.

Find root cause.

---

❌ ARP only exists in IPv4

Mostly true.

IPv6 uses NDP.

---

# Ultimate Troubleshooting Decision Tree ⭐⭐⭐⭐⭐

Remember forever.

```text
Problem

↓

Interface Up?

↓

IP Assigned?

↓

Route Exists?

↓

Gateway Reachable?

↓

Neighbor Table Healthy?

↓

MAC Learned?

↓

Traffic Flowing?
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Application

↓

DNS

↓

TCP

↓

IP

↓

Routing

↓

ARP

↓

MAC

↓

Ethernet

↓

NIC

↓

Internet
```

---

# WH Questions

## Why check ARP?

ARP enables local communication.

---

## Why check the gateway?

Everything internet-bound goes there.

---

## Why inspect neighbor states?

Linux reveals health information.

---

## Why do engineers use ip neigh?

It exposes ARP status.

---

## Why is troubleshooting order important?

Networking is layered.

---

# Key Takeaways

Remember forever.

```text
No Connectivity

↓

Interface

↓

IP

↓

Route

↓

Gateway

↓

ARP

↓

MAC

↓

Fix Problem
```

---

# Recommended Reading Order

```text
mac-address.md

↓

arp.md

↓

arp-visuals.md

↓

arp-security.md

↓

arp-vs-ndp.md

↓

arp-troubleshooting.md

↓

dns.md
```
