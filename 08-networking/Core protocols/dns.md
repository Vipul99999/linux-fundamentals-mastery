# DNS (Domain Name System)

> Learn how human-readable names become IP addresses and how Linux, cloud systems, containers, and modern infrastructure depend on DNS.

---

# Why Learn DNS?

Imagine if every website required memorizing IP addresses.

Instead of:

```text
google.com
```

you would need:

```text
142.250.x.x
```

Instead of:

```text
github.com
```

you would need:

```text
140.82.x.x
```

This would be impossible.

DNS solves this.

---

# The Big Question

When you type:

```text
google.com
```

Question:

```text
How does Linux know where Google is?
```

DNS solves this.

---

# Simple Definition

DNS is:

> A distributed naming system that translates domain names into IP addresses.

---

# Mental Model ⭐⭐⭐⭐⭐

Always remember:

```text
Human Language

↓

Machine Language

↓

DNS
```

---

# Simple Analogy

Think of your phone contacts.

Instead of remembering:

```text
+91-9876543210
```

You save:

```text
Mom
```

DNS works similarly.

Instead of:

```text
142.250.x.x
```

You type:

```text
google.com
```

---

# Big Picture

```text
Application

↓

DNS

↓

IP Address

↓

TCP

↓

Internet

↓

Destination
```

---

# End-To-End Example

Suppose:

```bash
curl github.com
```

Linux performs:

```text
Need IP

↓

DNS Lookup

↓

140.82.x.x

↓

TCP

↓

HTTPS

↓

GitHub
```

---

# Why DNS Exists

Without DNS:

```text
Billions of IPs

↓

Humans memorize them
```

Impossible.

---

# DNS Is A Giant Distributed Database

Not one server.

Millions of servers worldwide.

---

# Visualization

```text
Users

↓

ISPs

↓

DNS Servers

↓

Root Servers

↓

TLD Servers

↓

Authoritative Servers
```

---

# DNS Hierarchy ⭐⭐⭐⭐⭐

DNS is hierarchical.

```text
.

↓

com

↓

google

↓

www
```

---

# Domain Structure

Example:

```text
www.google.com
```

Visualization:

```text
www

↓

google

↓

com

↓

.
```

---

# Components

---

# Root Domain

```text
.
```

Top of DNS hierarchy.

---

# TLD (Top Level Domain)

Examples:

```text
.com

.org

.net

.edu

.io
```

---

# Second Level Domain

Examples:

```text
google

github

amazon
```

---

# Subdomain

Examples:

```text
www

api

docs
```

---

# DNS Resolution Journey ⭐⭐⭐⭐⭐

Suppose:

```text
github.com
```

Linux performs:

```text
Application

↓

Resolver

↓

DNS Server

↓

Root

↓

.com

↓

github

↓

IP

↓

Return
```

---

# Visual Journey

```text
Laptop

↓

DNS Resolver

↓

DNS Server

↓

Root Server

↓

.com Server

↓

GitHub Server

↓

IP Address

↓

Laptop
```

---

# Recursive DNS

Question:

```text
Can you find github.com for me?
```

DNS server does all the work.

---

# Iterative DNS

Question:

```text
Where should I ask next?
```

The client keeps asking.

---

# Recursive Visualization

```text
Laptop

↓

DNS Server

↓

Everything Else
```

---

# Iterative Visualization

```text
Laptop

↓

Root

↓

TLD

↓

Authoritative
```

---

# DNS Record Types ⭐⭐⭐⭐⭐

---

# A Record

Maps:

```text
Domain

↓

IPv4
```

Example:

```text
google.com

↓

142.x.x.x
```

---

# AAAA Record

Maps:

```text
Domain

↓

IPv6
```

---

# CNAME

Alias.

Example:

```text
www

↓

google.com
```

---

# MX

Mail server.

---

# TXT

Text metadata.

---

# NS

Name servers.

---

# PTR

Reverse DNS.

IP:

↓

Domain.

---

# Visual

```text
Domain

↓

DNS Records

↓

IP
```

---

# DNS Caching ⭐⭐⭐⭐⭐

DNS is expensive.

So systems cache results.

Without caching:

```text
google.com

↓

DNS

↓

DNS

↓

DNS

↓

DNS
```

Very expensive.

---

# With Cache

```text
DNS

↓

Stored

↓

Reuse
```

---

# TTL (Time To Live)

TTL determines cache duration.

Example:

```text
300 seconds
```

After expiry:

```text
Ask DNS again
```

---

# Linux Perspective ⭐⭐⭐⭐⭐

Linux does not directly know websites.

Linux asks a resolver.

---

# View DNS Servers

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 8.8.8.8
```

---

# Modern Linux

Many systems use:

```text
systemd-resolved
```

---

# View Resolver Status

```bash
resolvectl status
```

---

# Linux Internals ⭐⭐⭐⭐⭐

Suppose:

```bash
curl github.com
```

Linux performs:

```text
Application

↓

glibc Resolver

↓

DNS Resolver

↓

DNS Server

↓

IP

↓

Socket

↓

TCP

↓

Internet
```

---

# Internal Visualization

```text
User Space

Application

↓

glibc

↓

Resolver

===================

Kernel Space

Socket

↓

Network Stack

↓

Internet
```

---

# Browser Journey ⭐⭐⭐⭐⭐

Suppose:

```text
https://google.com
```

Journey:

```text
Browser

↓

DNS

↓

IP

↓

TCP

↓

TLS

↓

HTTP

↓

Google
```

DNS is the first step.

---

# Cloud Perspective ⭐⭐⭐⭐⭐

Cloud systems heavily use DNS.

Examples:

```text
Load Balancers

APIs

Services

Databases
```

---

# Docker Perspective ⭐⭐⭐⭐⭐

Docker has internal DNS.

Container:

```text
Container A

↓

Container B
```

by name.

---

# Kubernetes Perspective ⭐⭐⭐⭐⭐

Kubernetes has internal DNS.

Example:

```text
frontend.default.svc.cluster.local
```

DNS everywhere.

---

# Real Production Example

Architecture:

```text
User

↓

DNS

↓

CDN

↓

Load Balancer

↓

Application

↓

Database
```

---

# Security Perspective ⭐⭐⭐⭐⭐

DNS is heavily targeted.

Examples:

```text
DNS Spoofing

Cache Poisoning

DNS Tunneling

Amplification Attacks
```

---

# DNS Over HTTPS (DoH)

Traditional:

```text
DNS

↓

Plain Text
```

Modern:

```text
DNS

↓

HTTPS
```

Encrypted.

---

# DNS Over TLS (DoT)

DNS inside TLS.

---

# Troubleshooting ⭐⭐⭐⭐⭐

Problem:

```text
Website unavailable
```

Check:

```bash
dig google.com
```

---

Problem:

```text
Name resolution fails
```

Check:

```bash
cat /etc/resolv.conf
```

---

Problem:

```text
Slow websites
```

Check:

```text
DNS latency
```

---

# Linux Commands

---

# Query DNS

```bash
dig google.com
```

---

# Alternative

```bash
nslookup google.com
```

---

# Modern Tool

```bash
resolvectl query google.com
```

---

# Check Resolver

```bash
resolvectl status
```

---

# Common Misconceptions

❌ DNS hosts websites.

Wrong.

---

❌ DNS is one server.

Wrong.

---

❌ DNS only works on the internet.

Wrong.

Containers use DNS.

---

❌ DNS is always secure.

Wrong.

---

# Engineer Mental Model ⭐⭐⭐⭐⭐

Never think:

```text
Website

↓

Server
```

Think:

```text
Website Name

↓

DNS

↓

IP

↓

TCP

↓

Internet

↓

Server
```

---

# Visual Summary ⭐⭐⭐⭐⭐

```text
Human

↓

google.com

↓

DNS

↓

142.x.x.x

↓

TCP

↓

Internet

↓

Google
```

---

# WH Questions

## What is DNS?

Name-to-IP translator.

---

## Why does DNS exist?

Humans cannot memorize IPs.

---

## Why does Linux need DNS?

Applications need IP addresses.

---

## Why is DNS distributed?

Scalability.

---

## Why do engineers care?

Everything depends on DNS.

---

# Key Takeaways

Remember forever.

```text
Human Name

↓

DNS

↓

IP Address

↓

TCP

↓

Destination
```

---

# What's Next?

```text
dhcp.md
```

This is another extremely important file because it answers:

> How did my laptop automatically get an IP address when I joined WiFi?
