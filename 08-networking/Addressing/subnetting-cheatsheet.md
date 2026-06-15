# Subnetting Cheat Sheet

> Fast reference guide for subnetting, CIDR, Linux networking, cloud infrastructure, Docker, Kubernetes, and troubleshooting.

---

# Master Rule ⭐⭐⭐⭐⭐

Subnetting is NOT math.

Subnetting is:

```text
Network Design
```

Always think:

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

# Golden Rules ⭐⭐⭐⭐⭐

Rule 1

```text
Bigger Prefix

↓

Smaller Network
```

---

Rule 2

```text
More Network Bits

↓

Less Host Bits
```

---

Rule 3

```text
More Host Bits

↓

More Devices
```

---

Rule 4

```text
Same Subnet

↓

Direct Communication
```

---

Rule 5

```text
Different Subnet

↓

Use Gateway
```

---

# The Most Important Formula

Total Addresses:

```text
2^(Host Bits)
```

---

Usable Hosts:

```text
2^(Host Bits) - 2
```

Reserved:

```text
Network Address

+

Broadcast Address
```

---

# IPv4 Structure

```text
32 bits

=

4 octets

=

8 bits each
```

Visualization:

```text
192.168.1.25

↓

11000000

10101000

00000001

00011001
```

---

# CIDR Quick Table ⭐⭐⭐⭐⭐

| Prefix | Total Addresses | Usable Hosts |
|--------|----------------|--------------|
| /8 | 16,777,216 | 16,777,214 |
| /16 | 65,536 | 65,534 |
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |
| /31 | 2 | Special |
| /32 | 1 | Single Host |

---

# Prefix Memory Ladder ⭐⭐⭐⭐⭐

```text
/8

16 Million


/16

65 Thousand


/24

256


/25

128


/26

64


/27

32


/28

16


/29

8


/30

4
```

---

# Prefix To Subnet Mask Table ⭐⭐⭐⭐⭐

| Prefix | Subnet Mask |
|--------|-------------|
| /8 | 255.0.0.0 |
| /16 | 255.255.0.0 |
| /24 | 255.255.255.0 |
| /25 | 255.255.255.128 |
| /26 | 255.255.255.192 |
| /27 | 255.255.255.224 |
| /28 | 255.255.255.240 |
| /29 | 255.255.255.248 |
| /30 | 255.255.255.252 |
| /32 | 255.255.255.255 |

---

# Network vs Host ⭐⭐⭐⭐⭐

Example:

```text
192.168.1.25/24
```

Visualization:

```text
192.168.1 | 25

Network | Host
```

---

# Subnetting Workflow ⭐⭐⭐⭐⭐

Always ask these questions.

```text
How many devices?

↓

Future growth?

↓

Security requirements?

↓

Broadcast size?

↓

Choose subnet
```

---

# Fast Design Guide ⭐⭐⭐⭐⭐

| Requirement | Recommended |
|-------------|-------------|
| Home Network | /24 |
| Small Team | /26 |
| Tiny Team | /27 |
| Point-to-Point | /30 |
| Large Office | /24 |
| AWS Subnet | /24 |
| Kubernetes Cluster | /16 |

---

# Communication Decision Tree ⭐⭐⭐⭐⭐

```text
Destination

↓

Same Subnet?

↓

YES

↓

ARP

↓

MAC

↓

Direct Communication


NO

↓

Gateway

↓

Router

↓

Internet
```

---

# Linux Mental Model ⭐⭐⭐⭐⭐

Linux internally thinks:

```text
Application

↓

TCP

↓

IP

↓

Subnet Check

↓

Routing Table

↓

Gateway?

↓

ARP

↓

MAC

↓

NIC

↓

Internet
```

---

# Linux Commands Cheat Sheet

## Show interfaces

```bash
ip addr
```

---

## Show routes

```bash
ip route
```

---

## Show ARP cache

```bash
ip neigh
```

---

## Show interfaces only

```bash
ip link
```

---

## Test connectivity

```bash
ping google.com
```

---

## Show listening ports

```bash
ss -tuln
```

---

# Home Network Visual

```text
Internet

↓

Router

192.168.1.1

↓

Laptop

192.168.1.10

↓

Phone

192.168.1.20

↓

TV

192.168.1.30
```

Everything:

```text
192.168.1.0/24
```

---

# AWS Visual ⭐⭐⭐⭐⭐

```text
VPC

10.0.0.0/16

↓

Public

10.0.1.0/24

↓

Application

10.0.2.0/24

↓

Database

10.0.3.0/24
```

---

# Docker Visual ⭐⭐⭐⭐⭐

```text
Host

↓

docker0

172.17.0.1

↓

Container A

172.17.0.2

↓

Container B

172.17.0.3
```

---

# Kubernetes Visual ⭐⭐⭐⭐⭐

```text
Cluster

10.244.0.0/16

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

# Common Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
Cannot access printer
```

Check:

```text
Same subnet?
```

---

Problem:

```text
Cannot access internet
```

Check:

```text
Gateway configured?
```

---

Problem:

```text
Cloud resources cannot communicate
```

Check:

```text
CIDR overlap?
```

---

Problem:

```text
VPN fails
```

Check:

```text
Overlapping networks?
```

---

# CIDR Overlap Example ⭐⭐⭐⭐⭐

Bad:

```text
Office

10.0.0.0/16

↓

AWS

10.0.0.0/16
```

Problems:

```text
VPN

Peering

Routing

Failures
```

Good:

```text
Office

10.0.0.0/16

↓

AWS

10.100.0.0/16
```

---

# Security Design ⭐⭐⭐⭐⭐

Bad:

```text
Internet

↓

Database
```

---

Good:

```text
Internet

↓

Load Balancer

↓

Application

↓

Database
```

Different subnet per layer.

---

# Most Common Interview Questions ⭐⭐⭐⭐⭐

Question:

```text
Why do we subnet?
```

Answer:

```text
Reduce broadcasts

Improve security

Improve performance

Scale infrastructure
```

---

Question:

```text
What happens if two devices have the same IP?
```

Answer:

```text
IP conflict
```

---

Question:

```text
Why does Linux care about subnetting?
```

Answer:

```text
Routing decisions
```

---

Question:

```text
Why do cloud providers use subnetting?
```

Answer:

```text
Isolation

Security

Scalability
```

---

# Common Mistakes ⭐⭐⭐⭐⭐

❌ Memorizing formulas only

Wrong.

---

❌ Choosing huge networks unnecessarily

Wrong.

---

❌ Forgetting future growth

Wrong.

---

❌ Overlapping networks

Wrong.

---

❌ Thinking subnetting is math

Wrong.

---

# Ultimate Engineer Mental Model ⭐⭐⭐⭐⭐

Remember forever.

```text
How many devices?

↓

How much future growth?

↓

Security requirements?

↓

Broadcast requirements?

↓

Choose subnet

↓

Deploy

↓

Scale
```

---

# One Minute Revision ⭐⭐⭐⭐⭐

```text
Subnetting

↓

Split Network

↓

Reduce Broadcast

↓

Improve Security

↓

Improve Performance

↓

Scale Infrastructure


Bigger Prefix

↓

Smaller Network


Same Subnet

↓

Direct Communication


Different Subnet

↓

Gateway
```

---

# Recommended Reading Order

```text
subnetting.md

↓

subnetting-visuals.md

↓

subnetting-examples.md

↓

subnetting-cheatsheet.md

↓

cidr.md
```
