# DHCP (Dynamic Host Configuration Protocol)

> Learn how devices automatically receive network configuration, how Linux obtains IP addresses, and why modern infrastructure depends on DHCP.

---

# Why Learn DHCP?

Imagine every time you joined WiFi you had to manually configure:

```text
IP Address

Subnet Mask

Gateway

DNS Server
```

For every device.

Examples:

```text
Laptop

Phone

TV

Printer

Tablet

Camera

Server
```

This would be impossible at scale.

DHCP solves this.

---

# The Big Question

Suppose:

```text
You join WiFi
```

Question:

```text
How does your laptop suddenly know:

Its IP?

Its Gateway?

Its DNS Server?

Its Subnet?
```

Answer:

```text
DHCP
```

---

# Simple Definition

DHCP is:

> A protocol that automatically assigns network configuration information to devices.

---

# Mental Model ⭐⭐⭐⭐⭐

Think:

```text
New Device

↓

Needs Identity

↓

DHCP

↓

Configuration Received

↓

Ready To Communicate
```

---

# What Does DHCP Provide?

Usually:

```text
IP Address

↓

Subnet Mask

↓

Default Gateway

↓

DNS Server

↓

Lease Time
```

---

# Why DHCP Exists

Without DHCP:

```text
Employee 1

Configure manually

↓

Employee 2

Configure manually

↓

Employee 5000

Configure manually
```

Disaster.

---

# Big Picture

```text
New Device

↓

DHCP

↓

Gets Identity

↓

Can Use Network
```

---

# The Four-Step Process ⭐⭐⭐⭐⭐

This is the most important thing to remember.

```text
DORA
```

---

# D = Discover

Device says:

```text
Hello

Is there a DHCP server?
```

---

# O = Offer

Server replies:

```text
I can give you

192.168.1.20
```

---

# R = Request

Device says:

```text
I want that address.
```

---

# A = Acknowledge

Server says:

```text
Approved.
```

---

# DORA Visualization ⭐⭐⭐⭐⭐

```text
Laptop

↓

DHCP Discover

↓

DHCP Server

↓

DHCP Offer

↓

Laptop

↓

DHCP Request

↓

DHCP Server

↓

DHCP ACK

↓

Laptop Configured
```

---

# Engineer Memory Trick ⭐⭐⭐⭐⭐

Remember forever:

```text
DORA

↓

Discover

Offer

Request

Acknowledge
```

---

# Real WiFi Example

You enter a coffee shop.

```text
Laptop

↓

Connect WiFi

↓

DHCP Discover

↓

Router

↓

DHCP Offer

↓

Laptop

↓

DHCP Request

↓

Router

↓

DHCP ACK

↓

Internet Works
```

---

# Broadcast Behavior ⭐⭐⭐⭐⭐

Initially:

```text
Laptop

↓

No IP
```

Question:

```text
How can it find DHCP?
```

Answer:

Broadcast.

---

# Visualization

```text
Laptop

↓

DHCP Discover

↓

Broadcast

↓

Everyone Receives

↓

DHCP Server Responds
```

---

# DHCP Lease ⭐⭐⭐⭐⭐

IP addresses are not permanent.

They are borrowed.

This is called:

```text
Lease
```

---

# Visualization

```text
DHCP

↓

Assign IP

↓

Use Temporarily

↓

Renew

↓

Continue Using
```

---

# Why Use Leases?

Suppose:

```text
500 employees
```

but only:

```text
300 online
```

DHCP can recycle unused addresses.

Very efficient.

---

# Lease Lifecycle ⭐⭐⭐⭐⭐

```text
Assigned

↓

Halfway

↓

Renew

↓

Continue

↓

Expire

↓

Return To Pool
```

---

# DHCP Components

---

# DHCP Client

Device requesting configuration.

Examples:

```text
Laptop

Phone

Printer

Server
```

---

# DHCP Server

Assigns configuration.

Examples:

```text
Router

Windows Server

Linux Server

Cloud Infrastructure
```

---

# DHCP Relay

Helps communicate across different networks.

---

# Visualization

```text
Client

↓

Relay

↓

Server
```

---

# Why DHCP Relay Exists

DHCP broadcasts do not cross routers.

Large companies have many networks.

Relay solves this.

---

# Enterprise Example ⭐⭐⭐⭐⭐

```text
Floor 1

↓

Router

↓

Floor 2

↓

Router

↓

Central DHCP Server
```

Without relay:

```text
DHCP breaks
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux commonly uses:

```text
systemd-networkd

NetworkManager

dhclient

dhcpcd
```

to obtain addresses.

---

# Check Current Address

```bash
ip addr
```

---

# Check Routes

```bash
ip route
```

---

# View DNS

```bash
cat /etc/resolv.conf
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```text
Join WiFi
```

Linux performs:

```text
NIC Up

↓

DHCP Discover

↓

Broadcast

↓

Offer

↓

Request

↓

ACK

↓

Configure Interface

↓

Configure Routes

↓

Configure DNS

↓

Ready
```

---

# Internal Linux Visualization ⭐⭐⭐⭐⭐

```text
User Space

NetworkManager

↓

DHCP Client

=================

Kernel Space

Socket

↓

UDP

↓

IP

↓

NIC

↓

WiFi
```

---

# DHCP Uses UDP ⭐⭐⭐⭐⭐

DHCP does NOT use TCP.

It uses:

```text
UDP
```

Ports:

```text
67

DHCP Server


68

DHCP Client
```

---

# Visualization

```text
Laptop

UDP 68

↓

DHCP Server

UDP 67
```

---

# Why UDP?

Because:

```text
Fast

Simple

No connection setup
```

---

# Real Production Example ⭐⭐⭐⭐⭐

Company:

```text
5000 employees
```

DHCP automatically manages:

```text
Laptops

Phones

Printers

VoIP Phones

IoT Devices
```

without manual work.

---

# Docker Perspective ⭐⭐⭐⭐⭐

Docker has its own IP management.

Visualization:

```text
Docker

↓

Bridge

↓

IP Pool

↓

Containers
```

Similar concepts.

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Kubernetes typically uses:

```text
CNI Plugins
```

instead of DHCP.

But the idea is similar.

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud providers automatically assign:

```text
Private IPs

Public IPs

DNS

Routes
```

through infrastructure systems.

---

# Security Perspective ⭐⭐⭐⭐⭐

DHCP has risks.

Examples:

```text
Rogue DHCP Servers

DHCP Starvation

Misconfiguration
```

---

# Rogue DHCP Server

An unauthorized server appears.

Visualization:

```text
Laptop

↓

Wrong DHCP Server

↓

Bad Gateway

↓

No Connectivity
```

---

# DHCP Starvation

Attackers exhaust address pools.

Result:

```text
No IPs Available
```

---

# Modern Networks Use Protections

Examples:

```text
DHCP Snooping

Port Security

802.1X
```

---

# Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
Connected To WiFi

↓

No Internet
```

Check:

```bash
ip addr
```

Question:

```text
Did you receive an IP?
```

---

Problem:

```text
No Gateway
```

Check:

```bash
ip route
```

---

Problem:

```text
No DNS
```

Check:

```bash
cat /etc/resolv.conf
```

---

# Linux Troubleshooting Flow ⭐⭐⭐⭐⭐

```text
Connected?

↓

IP Assigned?

↓

Gateway Present?

↓

DNS Present?

↓

Internet Works?
```

---

# Linux Commands

Show interfaces:

```bash
ip link
```

---

Show IPs:

```bash
ip addr
```

---

Show routes:

```bash
ip route
```

---

Show DNS:

```bash
cat /etc/resolv.conf
```

---

Release lease (depends on client):

```bash
sudo dhclient -r
```

---

Renew lease:

```bash
sudo dhclient
```

---

# Packet Journey ⭐⭐⭐⭐⭐

Suppose:

```text
Turn WiFi ON
```

Linux internally:

```text
NIC

↓

DHCP Discover

↓

DHCP Offer

↓

DHCP Request

↓

DHCP ACK

↓

Configure Interface

↓

Configure Routing

↓

Configure DNS

↓

Ready
```

---

# Common Misconceptions

❌ DHCP only gives IPs.

Wrong.

It gives:

```text
IP

DNS

Gateway

Lease

Subnet
```

---

❌ DHCP and DNS are the same.

Wrong.

DHCP:

```text
Assigns identity
```

DNS:

```text
Finds websites
```

---

❌ DHCP only exists in homes.

Wrong.

It exists everywhere.

---

❌ DHCP crosses routers automatically.

Wrong.

Relays are needed.

---

# DHCP vs DNS ⭐⭐⭐⭐⭐

DHCP:

```text
Who am I?
```

DNS:

```text
Where is Google?
```

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Always think:

```text
New Device

↓

Needs Identity

↓

DHCP

↓

Identity Assigned

↓

Network Ready
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Connect Network

↓

DORA

↓

IP

↓

Gateway

↓

DNS

↓

Internet Ready
```

---

# WH Questions

## What is DHCP?

Automatic network configuration protocol.

---

## Why does DHCP exist?

Manual configuration doesn't scale.

---

## Why use leases?

Reuse addresses efficiently.

---

## Why use UDP?

Fast and simple.

---

## Why do Linux engineers care?

Every device depends on it.

---

## Why do enterprises use DHCP?

Scale.

---

# Key Takeaways

Remember forever.

```text
New Device

↓

DORA

↓

Identity Assigned

↓

Network Ready
```

---

# Recommended Reading Order

```text
arp.md

↓

dns.md

↓

dhcp.md

↓

icmp.md
```
