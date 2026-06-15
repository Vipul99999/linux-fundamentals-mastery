# Subnetting Examples

> Learn subnetting through practical examples, visuals, Linux perspectives, cloud infrastructure examples, and real-world engineering scenarios.

---

# Why This File Exists

Many people learn subnetting like this:

```text
192.168.1.0/26

255.255.255.192

64 addresses
```

Then memorize formulas.

This is the wrong approach.

Subnetting is not math.

Subnetting is:

```text
Network Design
```

This file teaches engineers how to think.

---

# Before Starting

Remember three things.

```text
Network

↓

Usable Hosts

↓

Broadcast
```

Every subnet has these.

---

# Example 1: Home Network

Suppose your WiFi router gives:

```text
192.168.1.0/24
```

Visual:

```text
Network

192.168.1.0

↓

Usable Hosts

192.168.1.1

↓

192.168.1.254

↓

Broadcast

192.168.1.255
```

---

# What Does /24 Mean?

```text
24 Network Bits

↓

8 Host Bits
```

Visualization:

```text
11111111

11111111

11111111

00000000
```

---

# Total Addresses

```text
2^8

=

256
```

---

# Why 254 Usable?

Two are reserved.

```text
Network Address

+

Broadcast Address
```

Formula:

```text
256 - 2

=

254
```

---

# Devices Example

```text
192.168.1.1

Router


192.168.1.10

Laptop


192.168.1.20

Phone


192.168.1.30

TV
```

---

# Visual

```text
Home Network

192.168.1.0/24

↓

Router

↓

Laptop

↓

Phone

↓

TV
```

---

# Example 2: Small Office

Suppose:

```text
100 employees
```

Need:

```text
100 hosts
```

Question:

```text
Which subnet?
```

---

# /25

Addresses:

```text
128
```

Usable:

```text
126
```

Good.

Visualization:

```text
192.168.1.0/25

↓

192.168.1.1

↓

192.168.1.126

↓

192.168.1.127
```

---

# Why Not /26?

```text
64 addresses
```

Too small.

---

# Engineer Mindset

Don't ask:

```text
Which prefix?
```

Ask:

```text
How many devices?
```

Then choose.

---

# Example 3: Split A /24 Into Two Networks

Original:

```text
192.168.1.0/24
```

Split:

```text
192.168.1.0/25

192.168.1.128/25
```

Visualization:

```text
Original

192.168.1.0

↓

Half 1

0-127

↓

Half 2

128-255
```

---

# Network 1

```text
192.168.1.0/25

Network

192.168.1.0

Usable

192.168.1.1

↓

192.168.1.126

Broadcast

192.168.1.127
```

---

# Network 2

```text
192.168.1.128/25

Network

192.168.1.128

Usable

192.168.1.129

↓

192.168.1.254

Broadcast

192.168.1.255
```

---

# Visual

```text
192.168.1.0/24

↓

Split

↓

192.168.1.0/25

↓

192.168.1.128/25
```

---

# Example 4: Engineering & HR Teams

Suppose:

```text
Engineering

60 Employees


HR

30 Employees
```

Subnet Design:

Engineering:

```text
192.168.1.0/26
```

HR:

```text
192.168.1.64/27
```

Visualization:

```text
Company

↓

Engineering

192.168.1.0/26

↓

HR

192.168.1.64/27
```

---

# Example 5: Datacenter Design

```text
Internet

↓

Load Balancer

↓

Application

↓

Database
```

Subnet Design:

```text
10.0.1.0/24

Load Balancer


10.0.2.0/24

Applications


10.0.3.0/24

Database
```

Visual:

```text
Internet

↓

10.0.1.0/24

↓

10.0.2.0/24

↓

10.0.3.0/24
```

---

# Example 6: AWS Example ⭐⭐⭐⭐⭐

AWS VPC:

```text
10.0.0.0/16
```

Subnets:

```text
10.0.1.0/24

Public


10.0.2.0/24

Application


10.0.3.0/24

Database
```

Visualization:

```text
AWS VPC

10.0.0.0/16

↓

Public

10.0.1.0/24

↓

Apps

10.0.2.0/24

↓

DB

10.0.3.0/24
```

---

# Example 7: Docker

Default Docker network:

```text
172.17.0.0/16
```

Containers:

```text
172.17.0.2

172.17.0.3

172.17.0.4
```

Visual:

```text
Docker Bridge

↓

Container A

↓

Container B

↓

Container C
```

---

# Example 8: Kubernetes

Cluster:

```text
10.244.0.0/16
```

Pods:

```text
10.244.1.10

10.244.1.20

10.244.2.10
```

Visual:

```text
Cluster

↓

Node 1

↓

Pods

↓

Node 2

↓

Pods
```

---

# Example 9: Same Subnet Communication ⭐⭐⭐⭐⭐

Laptop:

```text
192.168.1.10/24
```

Printer:

```text
192.168.1.20/24
```

Linux asks:

```text
Same subnet?
```

YES

↓

Direct communication.

Visual:

```text
Laptop

↓

Switch

↓

Printer
```

No router needed.

---

# Example 10: Different Subnet Communication ⭐⭐⭐⭐⭐

Laptop:

```text
192.168.1.10/24
```

Google DNS:

```text
8.8.8.8
```

Linux asks:

```text
Same subnet?
```

NO

↓

Use gateway.

Visual:

```text
Laptop

↓

Router

↓

Internet

↓

8.8.8.8
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Suppose:

```bash
ip addr
```

Output:

```text
192.168.1.10/24
```

Linux understands:

```text
Network Boundary

↓

255.255.255.0
```

---

# Linux Internal Flow ⭐⭐⭐⭐⭐

Suppose:

```bash
curl github.com
```

Linux internally:

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Subnet Check ⭐

↓

Same Network?

↓

Gateway?

↓

ARP

↓

NIC

↓

Internet
```

---

# Linux Commands

View interfaces:

```bash
ip addr
```

---

View routes:

```bash
ip route
```

---

View neighbors:

```bash
ip neigh
```

---

# Common Mistakes

---

# Mistake 1

Choosing too small a subnet.

Bad:

```text
60 users

↓

/27

30 usable
```

---

# Mistake 2

Choosing huge networks unnecessarily.

Bad:

```text
10 users

↓

/16
```

Wasteful.

---

# Mistake 3

Overlapping networks.

Bad:

```text
10.0.0.0/16

10.0.0.0/16
```

Problems:

```text
VPN breaks

Cloud peering breaks

Routing breaks
```

---

# Engineer Mental Model

Always think:

```text
How many devices?

↓

How much future growth?

↓

Security?

↓

Broadcast size?

↓

Choose subnet
```

---

# Useful Prefix Table

| Prefix | Addresses | Usable |
|--------|-----------|--------|
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |

---

# Visual Summary

```text
Large Network

↓

Split

↓

Smaller Networks

↓

Less Broadcast

↓

More Security

↓

Better Performance
```

---

# WH Questions

## Why split networks?

Performance.

---

## Why reduce broadcasts?

Efficiency.

---

## Why does Linux care?

Routing decisions.

---

## Why do cloud providers use subnetting?

Isolation.

---

## Why do containers use subnetting?

Network separation.

---

## Why do engineers use subnetting?

Scalability.

---

# Key Takeaways

Remember forever.

```text
Large Network

↓

Split

↓

Organize

↓

Secure

↓

Scale
```

---

# Related Files

Read next:

```text
subnetting-visuals.md ⭐⭐⭐⭐⭐

subnetting-cheatsheet.md ⭐⭐⭐⭐⭐

cidr.md
```
