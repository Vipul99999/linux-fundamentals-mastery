# ARP (Address Resolution Protocol)

> Learn how Linux converts IP addresses into MAC addresses and how devices discover each other inside local networks.

---

# Why Learn ARP?

ARP answers one of the most important networking questions.

Suppose:

```text
Laptop

192.168.1.10
```

wants to communicate with:

```text
Printer

192.168.1.20
```

Question:

```text
I know the IP.

But how do I physically find the device?
```

ARP solves this.

Without ARP:

```text
Ethernet

↓

Cannot work

↓

Local networking fails
```

---

# The Big Question

If IP already exists:

```text
192.168.1.20
```

Why isn't that enough?

Because:

```text
IP

↓

Logical identity


MAC

↓

Physical local identity
```

Linux cannot physically deliver Ethernet frames using IP addresses.

It needs MAC addresses.

---

# Simple Definition

ARP is:

> A protocol that maps an IPv4 address to a MAC address inside a local network.

---

# Mental Model ⭐⭐⭐⭐⭐

Always remember:

```text
I know the IP

↓

I need the MAC

↓

ARP
```

---

# Where Does ARP Operate?

ARP sits between:

```text
Layer 2

↓

Data Link


Layer 3

↓

Network
```

Think of it as a translator.

---

# Visual

```text
Application

↓

TCP

↓

IP

↓

ARP ⭐

↓

MAC

↓

Ethernet

↓

Destination
```

---

# House Delivery Analogy

Imagine:

```text
Street Address

↓

House Door
```

IP:

```text
Street Address
```

MAC:

```text
House Door
```

ARP says:

```text
Which door belongs to this address?
```

---

# Simple Scenario

Your laptop:

```text
192.168.1.10
```

Printer:

```text
192.168.1.20
```

Linux asks:

```text
Who owns 192.168.1.20 ?
```

ARP resolves it.

---

# ARP Workflow ⭐⭐⭐⭐⭐

```text
Laptop

↓

ARP Request

↓

Broadcast

↓

Everyone Receives

↓

Printer Replies

↓

MAC Learned

↓

Communication Begins
```

---

# Visual Flow

```text
Laptop

↓

Who has 192.168.1.20 ?

↓

Switch

↓

Everyone Receives

↓

Printer

↓

I have it

↓

AA:BB:CC:DD:EE:FF

↓

Laptop Stores It
```

---

# Step By Step Internal Process

---

# Step 1

Linux wants to send data.

```text
192.168.1.20
```

---

# Step 2

Linux checks:

```text
ARP Cache
```

Question:

```text
Do I already know the MAC?
```

---

# Step 3

If NO:

Broadcast request.

```text
Who has 192.168.1.20 ?
```

---

# Step 4

Destination responds.

```text
192.168.1.20

↓

AA:BB:CC:DD:EE:FF
```

---

# Step 5

Linux stores it.

```text
IP

↓

MAC
```

---

# Step 6

Linux sends Ethernet frame.

---

# ARP Request Visualization ⭐⭐⭐⭐⭐

```text
Laptop

192.168.1.10

↓

ARP Request

↓

FF:FF:FF:FF:FF:FF

(Broadcast)

↓

Switch

↙ ↓ ↓ ↓ ↘

Phone

Printer

TV

Camera
```

---

# ARP Reply Visualization

```text
Printer

↓

ARP Reply

↓

AA:BB:CC:DD:EE:FF

↓

Laptop
```

---

# Broadcast MAC Address

ARP requests use:

```text
FF:FF:FF:FF:FF:FF
```

Meaning:

```text
Everybody Listen
```

---

# ARP Cache ⭐⭐⭐⭐⭐

Linux remembers results.

Without caching:

```text
ARP

↓

ARP

↓

ARP

↓

ARP
```

Too expensive.

---

# Visualization

```text
IP

↓

MAC

↓

Saved
```

---

# Example Cache

```text
192.168.1.1

↓

AA:BB:CC:DD:EE:01


192.168.1.20

↓

AA:BB:CC:DD:EE:02
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux heavily relies on ARP.

Before sending local traffic:

Linux checks:

```text
ARP Cache
```

first.

---

# View ARP Cache

```bash
ip neigh
```

Example:

```text
192.168.1.1

lladdr

AA:BB:CC:DD:EE:01
```

---

# Old Command

```bash
arp -a
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```bash
curl github.com
```

Linux internally performs:

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Routing Table

↓

Same Network?

↓

YES

↓

ARP ⭐

↓

MAC Discovery

↓

Ethernet Frame

↓

NIC

↓

Wire
```

---

# Internal Linux Visualization

```text
User Space

Application

↓

====================

Kernel Space

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

Driver

↓

NIC
```

---

# ARP Packet Structure

ARP contains:

```text
Hardware Type

Protocol Type

Hardware Length

Protocol Length

Operation

Sender MAC

Sender IP

Target MAC

Target IP
```

---

# Visualization

```text
┌────────────────────┐
│ Hardware Type      │
├────────────────────┤
│ Protocol Type      │
├────────────────────┤
│ Sender MAC         │
├────────────────────┤
│ Sender IP          │
├────────────────────┤
│ Target MAC         │
├────────────────────┤
│ Target IP          │
└────────────────────┘
```

---

# Same Network Example ⭐⭐⭐⭐⭐

Laptop:

```text
192.168.1.10
```

Printer:

```text
192.168.1.20
```

Linux:

```text
Same subnet?

↓

YES

↓

ARP
```

---

# Different Network Example ⭐⭐⭐⭐⭐

Laptop:

```text
192.168.1.10
```

Google DNS:

```text
8.8.8.8
```

Linux:

```text
Same subnet?

↓

NO

↓

Gateway
```

Linux ARPs for:

```text
Router MAC
```

NOT:

```text
8.8.8.8 MAC
```

This is extremely important.

---

# Visualization ⭐⭐⭐⭐⭐

Wrong:

```text
Laptop

↓

ARP Google
```

No.

Correct:

```text
Laptop

↓

ARP Router

↓

Router

↓

Internet

↓

Google
```

---

# Why ARP Is Local Only

Routers do NOT forward ARP packets.

ARP remains inside:

```text
Broadcast Domain
```

---

# Visual

```text
Room 1

↓

ARP Works

↓

Router

↓

Room 2

↓

Different ARP Domain
```

---

# Modern Infrastructure Usage

---

# Home Networks

Uses ARP heavily.

---

# Office Networks

Uses ARP heavily.

---

# Datacenters

Uses ARP heavily.

---

# Docker

Containers use ARP.

---

# Kubernetes

Pods use ARP.

---

# Virtual Machines

Use ARP.

---

# AWS

Still relies on ARP internally.

---

# Security Perspective ⭐⭐⭐⭐⭐

ARP is insecure by design.

There is no authentication.

This causes attacks.

---

# ARP Spoofing

Attacker says:

```text
I am the router
```

Victim believes it.

---

# Visualization

Normal:

```text
Laptop

↓

Router

↓

Internet
```

Attack:

```text
Laptop

↓

Attacker

↓

Router

↓

Internet
```

---

# This Enables

```text
Man In The Middle

Packet Sniffing

Traffic Manipulation
```

---

# Common ARP Attacks

```text
ARP Spoofing

ARP Poisoning

MITM
```

---

# Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
Cannot access printer
```

Check:

```bash
ip neigh
```

---

Problem:

```text
Cannot access gateway
```

Check:

```bash
ip neigh
```

---

Problem:

```text
Intermittent connectivity
```

Check:

```text
Duplicate IP?

ARP poisoning?
```

---

# Linux Commands Cheat Sheet

View ARP cache:

```bash
ip neigh
```

---

Delete entry:

```bash
sudo ip neigh del 192.168.1.20 dev eth0
```

---

Flush cache:

```bash
sudo ip neigh flush all
```

---

Monitor:

```bash
ip monitor neigh
```

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
IP

↓

Destination
```

Think:

```text
IP

↓

Need MAC

↓

ARP

↓

Ethernet

↓

Destination
```

---

# Common Misconceptions

❌ ARP works across the internet.

Wrong.

ARP is local.

---

❌ ARP finds websites.

Wrong.

DNS does that.

---

❌ Linux ARPs for Google.

Wrong.

Linux ARPs for the gateway.

---

❌ ARP is secure.

Wrong.

ARP has no authentication.

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Know IP

↓

Need MAC

↓

ARP

↓

Ethernet

↓

Destination
```

---

# WH Questions

## What is ARP?

IP to MAC mapping protocol.

---

## Why does ARP exist?

IP alone cannot physically deliver Ethernet frames.

---

## Why does Linux need ARP?

To send local traffic.

---

## Why is ARP local only?

Routers don't forward ARP broadcasts.

---

## Why is ARP insecure?

No authentication.

---

## Why do engineers care?

Everything local networking depends on ARP.

---

# Key Takeaways

Remember forever.

```text
IP

↓

Need MAC

↓

ARP

↓

Ethernet

↓

Destination
```

---

# What's Next?

```text
dns.md
```

This is another extremely important file because it answers:

> How does `google.com` become an IP address?
