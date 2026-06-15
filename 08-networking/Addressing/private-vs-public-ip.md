# Private IP vs Public IP

> Learn why your devices have two different identities, how routers translate addresses, and how Linux, cloud platforms, and modern infrastructure use private and public networking.

---

# Why Learn This?

Imagine every house on Earth used:

```text
House #1
```

Postal delivery would become impossible.

Networking has the same problem.

We need:

```text
Global identities

+

Local identities
```

This is exactly why private and public IPs exist.

Without this concept:

❌ Home networks wouldn't scale

❌ WiFi routers wouldn't work

❌ Cloud networking would be difficult

❌ IPv4 would have run out much earlier

---

# The Big Question

When you open:

```text
https://google.com
```

Your laptop may have:

```text
192.168.1.10
```

But Google never sees:

```text
192.168.1.10
```

Why?

Because that is a private IP.

---

# Simple Definitions

## Private IP

Used inside local networks.

Not accessible directly from the internet.

---

## Public IP

Globally unique.

Accessible across the internet.

---

# House Analogy

Inside your apartment:

```text
Room 101

Room 102

Room 103
```

Outside world:

```text
Apartment Building Address
```

Networking:

```text
Private IPs

↓

Public IP
```

---

# Big Picture

```text
Laptop

192.168.1.10

↓

Router

↓

49.x.x.x

↓

Internet

↓

Google
```

---

# Why Were Private IPs Invented?

IPv4 has limits.

Total:

```text
4.3 Billion Addresses
```

This is not enough.

Imagine:

```text
8 Billion Humans

↓

Multiple Devices Each

↓

Huge Problem
```

Private IPs help conserve addresses.

---

# The Solution

Instead of:

```text
Every device

↓

Public IP
```

Use:

```text
Many private devices

↓

One public IP
```

---

# Visualization

Without Private IPs:

```text
Laptop

↓

Public IP

Phone

↓

Public IP

TV

↓

Public IP

Printer

↓

Public IP
```

Huge waste.

---

With Private IPs:

```text
Laptop

↓

Phone

↓

TV

↓

Printer

↓

Router

↓

One Public IP
```

Much better.

---

# Private IPv4 Ranges

These are reserved.

---

# Class A

```text
10.0.0.0

↓

10.255.255.255
```

---

# Class B

```text
172.16.0.0

↓

172.31.255.255
```

---

# Class C

```text
192.168.0.0

↓

192.168.255.255
```

---

# Visualization

```text
10.x.x.x

172.16-31.x.x

192.168.x.x
```

Everything else is generally public.

---

# Why Are These Reserved?

Routers on the internet refuse to route them globally.

They are only for local use.

---

# Public IP Addresses

Examples:

```text
8.8.8.8

1.1.1.1

142.x.x.x
```

Characteristics:

```text
Globally Unique

Internet Accessible

Routable
```

---

# Real World Example

Your home network.

```text
Laptop

192.168.1.10

↓

Router

192.168.1.1

↓

Public IP

49.x.x.x

↓

Internet
```

---

# How Does This Work?

Through NAT.

---

# NAT (Network Address Translation)

Router translates addresses.

Visualization:

```text
192.168.1.10

↓

49.x.x.x

↓

Internet
```

---

# Return Journey

```text
Google

↓

49.x.x.x

↓

Router

↓

192.168.1.10
```

Router remembers who initiated the request.

---

# NAT Table Visualization

```text
Private IP

↓

Port

↓

Public IP

↓

Port
```

Example:

```text
192.168.1.10:54321

↓

49.x.x.x:60001
```

---

# Why Ports Matter

One public IP may serve many devices.

Router distinguishes devices using ports.

---

# Visualization

```text
Laptop

↓

192.168.1.10

↓

Port 50001


Phone

↓

192.168.1.20

↓

Port 50002


Router

↓

49.x.x.x
```

---

# End-To-End Example

Suppose:

```text
Laptop

192.168.1.10
```

opens:

```text
github.com
```

Flow:

```text
Laptop

↓

192.168.1.10

↓

Router

↓

49.x.x.x

↓

Internet

↓

GitHub
```

---

# Why Can't My Friend Access My Laptop?

Because:

```text
192.168.1.10
```

exists only inside your local network.

The internet does not know it.

---

# Router Perspective

Router has two sides.

---

# LAN Side

```text
192.168.1.1
```

---

# WAN Side

```text
49.x.x.x
```

---

# Visualization

```text
LAN

↓

Router

↓

WAN

↓

Internet
```

---

# Linux Perspective ⭐⭐⭐

Linux can work with both.

Examples:

```text
Laptop

Private IP

↓

Cloud VM

Public IP
```

---

# View IP Addresses

```bash
ip addr
```

---

# View Routes

```bash
ip route
```

---

# View Public IP

```bash
curl ifconfig.me
```

or

```bash
curl ipinfo.io/ip
```

---

# Linux Networking Internals ⭐⭐⭐

Suppose:

```text
curl github.com
```

Linux performs:

```text
Application

↓

Socket

↓

TCP

↓

IP

↓

Router

↓

NAT

↓

Internet
```

---

# Internal Journey Visualization

```text
curl

↓

DNS

↓

Socket

↓

TCP

↓

Private IP

↓

Router

↓

Public IP

↓

Internet
```

---

# Cloud Perspective ⭐⭐⭐

Cloud environments often combine both.

Example:

```text
Public Load Balancer

↓

Private Application Servers

↓

Private Database
```

---

# Visualization

```text
Internet

↓

Public Load Balancer

↓

Private App

↓

Private DB
```

---

# Docker Perspective

Containers often use:

```text
172.x.x.x
```

internally.

---

# Kubernetes Perspective

Pods often use:

```text
10.x.x.x
```

internally.

---

# Production Systems

Modern infrastructures heavily rely on private networks.

Examples:

```text
AWS VPC

Azure VNet

GCP VPC

Docker Networks

Kubernetes Clusters
```

---

# Security Perspective

Good design:

```text
Internet

↓

Public Load Balancer

↓

Private Services

↓

Private Database
```

Bad design:

```text
Internet

↓

Database
```

Never expose databases publicly unless necessary.

---

# Troubleshooting

## Problem

Internet not working.

Check:

```bash
ip addr
```

---

## Problem

Local network works.

Internet doesn't.

Check:

```text
Router

Gateway

NAT
```

---

## Problem

Cannot access cloud VM.

Check:

```text
Public IP?

Firewall?

Security Group?
```

---

# Troubleshooting Flow

```text
No Internet

↓

Private IP Exists?

↓

Gateway Exists?

↓

Router Working?

↓

Public IP Exists?

↓

Internet Reachable?
```

---

# Engineer Mental Model

Never think:

```text
Laptop

↓

Internet
```

Think:

```text
Laptop

↓

Private IP

↓

Router

↓

NAT

↓

Public IP

↓

Internet
```

---

# Visual Summary

```text
Inside Network

↓

Private IP


Outside Network

↓

Public IP


Translator

↓

Router (NAT)
```

---

# WH Questions

## What is a private IP?

Local network identity.

---

## What is a public IP?

Global internet identity.

---

## Why do private IPs exist?

To conserve IPv4 addresses.

---

## Why do routers exist?

To connect local networks to the internet.

---

## Why does NAT exist?

To translate addresses.

---

## Why can't people access my laptop?

Private IPs are not internet routable.

---

# Key Takeaways

Remember this forever.

```text
Private IP

↓

Router

↓

NAT

↓

Public IP

↓

Internet
```

---

# What's Next?

```text
mac-address.md
```

This is where networking becomes much clearer because you'll finally understand:

> Why do we need MAC addresses if IP addresses already exist?
