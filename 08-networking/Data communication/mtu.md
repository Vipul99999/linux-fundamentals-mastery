# MTU (Maximum Transmission Unit)

> Learn why packet sizes have limits, how Linux handles oversized packets, and why MTU is one of the most overlooked causes of networking problems.

---

# Why Learn MTU?

Imagine sending a 1 GB file.

Do networks send:

```text
1 giant packet
```

No.

That would be extremely inefficient.

Networks send many smaller packets.

But another question appears.

> How big should one packet be?

MTU answers this.

---

# Simple Definition

MTU is:

> The maximum amount of data a network interface can transmit in a single frame without fragmentation.

---

# Simple Analogy

Imagine roads.

Small cars:

```text
Easy movement
```

Huge trucks:

```text
May not fit everywhere
```

Networks work similarly.

Large packets cannot always travel through every network.

---

# What Problem Does MTU Solve?

Without limits:

```text
Gigantic packets

↓

Memory issues

↓

Congestion

↓

Retransmission becomes expensive

↓

Performance degrades
```

MTU standardizes transmission sizes.

---

# The Big Picture

```text
Application Data

↓

TCP Segment

↓

IP Packet

↓

Ethernet Frame

↓

Physical Network
```

MTU usually limits the size at Layer 2.

---

# Typical MTU Values

| Technology | Typical MTU |
|------------|-------------|
| Ethernet | 1500 |
| PPPoE | 1492 |
| VPN | 1400-1450 |
| Jumbo Frames | 9000 |
| Docker Networks | Usually 1500 |
| Cloud Networks | Depends on provider |

---

# Why Is Ethernet Usually 1500?

Historically:

```text
Balance between:

Efficiency

Performance

Memory usage

Hardware limitations
```

1500 became a widely accepted standard.

---

# Important Clarification

Many beginners think:

```text
MTU = Entire Frame Size
```

Not exactly.

1500 bytes usually refers to:

```text
IP Payload
```

Ethernet headers are separate.

---

# Ethernet Visualization

```text
┌────────────────────┐
│ Ethernet Header    │ 14 bytes
├────────────────────┤
│ IP Packet          │ <=1500 bytes
└────────────────────┘
```

Total transmitted size becomes larger.

---

# What Happens If Data Is Larger?

Suppose:

```text
3000 bytes
```

MTU:

```text
1500 bytes
```

Result:

```text
Packet 1

1500 bytes

+

Packet 2

1500 bytes
```

Data is split.

---

# Visualization

```text
3000 bytes

↓

1500

↓

1500
```

---

# Fragmentation

Fragmentation means:

> Breaking large packets into smaller pieces.

---

# Visualization

```text
Large Packet

↓

Fragment 1

↓

Fragment 2

↓

Fragment 3
```

---

# Why Is Fragmentation Bad?

Fragmentation causes:

```text
Extra CPU work

↓

Extra memory usage

↓

More retransmissions

↓

Performance degradation
```

Modern systems try to avoid it.

---

# Fragmentation Example

Suppose:

```text
2000 bytes
```

Travels into:

```text
1500 MTU network
```

Result:

```text
Fragment 1

1500

Fragment 2

500
```

---

# Path MTU

A packet may travel through multiple networks.

Example:

```text
Laptop

↓

Router

↓

ISP

↓

VPN

↓

Cloud

↓

Google
```

Every network may have different MTU values.

The smallest MTU becomes important.

---

# Path MTU Visualization

```text
1500

↓

1500

↓

1450

↓

1400

↓

1500
```

Effective MTU:

```text
1400
```

---

# Path MTU Discovery (PMTUD)

PMTUD helps devices discover the smallest MTU.

---

# How It Works

Sender says:

```text
Do not fragment this packet.
```

If a router cannot handle it:

```text
ICMP

Packet Too Big
```

Sender reduces packet size.

---

# Visualization

```text
Sender

↓

1500 bytes

↓

Router

↓

Too Big

↓

ICMP Response

↓

Reduce Size

↓

1400 bytes
```

---

# Why PMTUD Is Important

Without it:

```text
Random failures

Slow websites

Broken VPNs

Intermittent issues
```

may happen.

---

# The Famous MTU Black Hole Problem

This is a real production issue.

Scenario:

```text
Sender

↓

1500 packet

↓

VPN

↓

1400 MTU

↓

ICMP blocked

↓

Packet dropped
```

Result:

```text
Website partially loads

↓

Hangs forever
```

Very common.

---

# Symptoms Of MTU Problems

Examples:

```text
Small websites work

Large websites fail

SSH freezes

VPN unstable

Uploads fail

Downloads fail

Intermittent timeouts
```

---

# Linux Perspective ⭐⭐⭐

Linux heavily relies on MTU.

Every network interface has one.

---

# View MTU

```bash
ip link
```

Example:

```text
2: eth0

mtu 1500
```

---

# Alternative

```bash
ifconfig
```

Example:

```text
eth0

MTU:1500
```

---

# Change MTU Temporarily

```bash
sudo ip link set dev eth0 mtu 1400
```

---

# Verify

```bash
ip link show eth0
```

---

# Linux Networking Internals

Linux packet journey:

```text
Application

↓

Socket API

↓

Kernel TCP/IP Stack

↓

Routing

↓

Interface

↓

MTU Check ⭐

↓

NIC Driver

↓

NIC Hardware
```

---

# Internal Linux Decision Process

Linux checks:

```text
Packet Size

↓

Can interface handle it?

↓

YES

↓

Transmit


NO

↓

Fragment or Drop
```

Depending on configuration.

---

# Docker & MTU

Docker creates virtual networks.

Sometimes MTU mismatches occur.

Example:

```text
Host

1500

↓

Docker

1500

↓

VPN

1400
```

Problems appear.

---

# Kubernetes & MTU

Kubernetes networking layers:

```text
Pod

↓

Virtual Interface

↓

Overlay Network

↓

Host Network

↓

Cloud Network
```

Each layer may reduce MTU.

---

# VXLAN Problem

VXLAN adds extra headers.

Example:

```text
1500

↓

VXLAN Header

↓

Effective MTU

1450
```

This is very common.

---

# Cloud Providers Care About MTU

Examples:

```text
AWS

Azure

GCP
```

Virtual networking may reduce available MTU.

---

# Jumbo Frames

Some datacenters use:

```text
9000 bytes
```

instead of:

```text
1500
```

Advantages:

```text
Less CPU

Less overhead

Better throughput
```

Requirements:

```text
Every device must support it
```

---

# Visualization

Normal:

```text
100 packets
```

Jumbo:

```text
20 packets
```

for the same data.

---

# Linux Engineer Troubleshooting Flow

Suppose:

```text
VPN users report slow websites.
```

Checklist:

```text
VPN?

↓

Overlay network?

↓

Cloud network?

↓

MTU mismatch?

↓

ICMP blocked?
```

---

# Useful Linux Commands

---

# Show interfaces

```bash
ip link
```

---

# Show detailed info

```bash
ip -details link
```

---

# Test MTU

Linux:

```bash
ping -M do -s 1472 google.com
```

Explanation:

```text
1472

+

28 bytes headers

=

1500
```

---

# Real World Example

User:

```text
Cannot upload large files.
```

Small requests work.

Diagnosis:

```text
MTU mismatch
```

Very common.

---

# Security Considerations

Attackers may abuse fragmentation.

Examples:

```text
Fragmentation attacks

Evasion techniques

Firewall bypass attempts
```

Modern systems inspect fragments carefully.

---

# Engineer Mental Model

Never think:

```text
Packet

↓

Destination
```

Think:

```text
Packet

↓

Can every network carry it?

↓

Yes

↓

Deliver


No

↓

Reduce size
```

---

# WH Questions

## What is MTU?

Maximum transferable size before fragmentation.

---

## Why does MTU exist?

Networks have physical limitations.

---

## Why is fragmentation bad?

Extra work and reduced performance.

---

## Why do VPNs cause MTU issues?

Additional headers reduce available space.

---

## Why does Kubernetes care about MTU?

Overlay networks add overhead.

---

## Why do Linux engineers care?

MTU mismatches create difficult production problems.

---

# Key Takeaways

Remember this forever.

```text
Big Data

↓

Packets

↓

MTU Check

↓

Fragment?

↓

Transmit
```

---

# What's Next?

```text
# Addressing

ip-addressing.md
```

Now we begin one of the most important networking sections.

You will learn:

```text
Who am I?

Who are you?

Where should packets go?
```

This is where networking starts becoming much more practical.
