# Routing Security

# Why This File Matters

Most beginners think:

> Security is firewalls.

This is one of the biggest misconceptions in infrastructure engineering.

Security is much broader.

Security is:

```text
Who can talk?

↓

Who should not talk?

↓

How does traffic move?

↓

Who controls traffic movement?

↓

Can traffic be redirected?

↓

Can traffic be intercepted?
```

Routing sits at the center of all these questions.

If an attacker controls routing, they often control the infrastructure.

This is why routing security is foundational for:

- Linux Engineers
- DevOps Engineers
- SREs
- Backend Engineers
- Cloud Engineers
- Platform Engineers
- Security Engineers

This file connects networking with security engineering.

---

# Learning Goals

After reading this file, you should be able to answer:

- Why is routing a security problem?
- How can attackers abuse routing?
- What is ARP poisoning?
- What is route hijacking?
- What is BGP hijacking?
- What are blackhole routes?
- How do cloud route mistakes expose systems?
- How do engineers secure traffic paths?
- How does Zero Trust relate to routing?

---

# The Core Security Principle

Everything eventually becomes:

> Can packets go somewhere they shouldn't?

If yes:

You have a security problem.

---

# The Three Questions Engineers Must Always Ask

Question 1:

```text
Who can initiate communication?
```

Question 2:

```text
Who can receive communication?
```

Question 3:

```text
Can traffic be redirected?
```

---

# The Packet Is The Asset

Beginners think:

```text
Database is the asset.
```

Reality:

```text
Traffic is the asset.
```

Attackers often steal:

```text
Credentials

Sessions

Tokens

Secrets

API keys
```

These are all packets.

Protecting packets protects infrastructure.

---

# Security Starts With Traffic Boundaries

Imagine this infrastructure.

```text
Internet

↓

Load Balancer

↓

Application Servers

↓

Databases
```

Should databases talk directly to internet?

No.

Routing should enforce boundaries.

---

# Good Architecture

```text
Internet

↓

Load Balancer

↓

Application Layer

↓

Database Layer
```

Database never exposed.

---

# Bad Architecture

```text
Internet

↓

Database
```

This still happens surprisingly often.

Especially in cloud environments.

---

# Threat 1: ARP Poisoning

ARP asks:

```text
Who owns this IP?
```

Attackers can lie.

---

# Normal Operation

```text
Laptop

↓

Who owns 192.168.1.1?

↓

Router

↓

I do.
```

---

# Attack

Attacker says:

```text
I am 192.168.1.1
```

Laptop believes it.

Traffic gets redirected.

---

# Visualization

```text
Laptop

↓

Attacker

↓

Router
```

Attacker sits in the middle.

This is:

> Man-In-The-Middle (MITM)

---

# What Attackers Can Do

They may:

```text
Read traffic

Modify traffic

Drop traffic

Record credentials

Hijack sessions
```

---

# Detect ARP Poisoning

Check:

```bash
ip neigh
```

Unexpected MAC changes?

Investigate.

Tools:

```bash
arp -a
```

or

```bash
ip neigh
```

---

# Protection

Use:

```text
Dynamic ARP Inspection

Static ARP entries

Network segmentation

TLS encryption
```

---

# Threat 2: Route Hijacking

Suppose:

Normal route:

```text
Server

↓

Router A

↓

Database
```

Attacker changes route.

```text
Server

↓

Attacker

↓

Database
```

Traffic is now compromised.

---

# How Route Hijacking Happens

Sources:

```text
Compromised router

Compromised Linux host

Cloud misconfiguration

BGP attacks
```

---

# Threat 3: BGP Hijacking

This is internet-scale.

BGP powers the internet.

Imagine:

Google owns:

```text
8.8.8.0/24
```

Attacker announces:

```text
I own:

8.8.8.0/24
```

Traffic gets redirected.

This has happened many times.

---

# Visualization

```text
User

↓

ISP

↓

Attacker

↓

Google
```

Very dangerous.

---

# Real Incidents

Historically:

```text
Google traffic

YouTube traffic

Crypto traffic
```

have all experienced BGP hijacking incidents.

---

# BGP Security Mechanisms

Modern internet uses:

```text
RPKI

Route Filtering

Prefix Validation
```

We'll learn these later.

---

# Threat 4: Cloud Route Exposure

This is extremely common.

AWS example.

Private subnet:

```text
10.0.2.0/24
```

Accidental route:

```text
0.0.0.0/0

↓

Internet Gateway
```

Infrastructure becomes public.

---

# Cloud Security Rule

Always verify:

```text
Subnet

↓

Route Table

↓

Security Group

↓

NACL
```

Never trust defaults.

---

# Threat 5: Kubernetes Network Exposure

Bad setup:

```text
Internet

↓

Pod

↓

Database
```

Pods become publicly reachable.

Attackers scan constantly.

---

# Good Kubernetes Design

```text
Internet

↓

Ingress

↓

Services

↓

Pods

↓

Databases
```

Controlled traffic.

---

# Threat 6: Lateral Movement

Attackers rarely attack every server individually.

Instead:

```text
Compromise one server

↓

Move sideways
```

This is:

> Lateral Movement

---

# Example

Attacker compromises:

```text
App Server
```

Can it reach:

```text
Database?
```

Can it reach:

```text
Kubernetes API?
```

Can it reach:

```text
Redis?
```

If yes:

Large problem.

---

# Segmentation Solves This

Bad:

```text
Everything talks to everything.
```

Good:

```text
Internet

↓

DMZ

↓

Application

↓

Database

↓

Management
```

Separate zones.

---

# Visual Example

```text
Internet

↓

Zone A

↓

Zone B

↓

Zone C
```

Restricted communication.

---

# Principle Of Least Connectivity

We know:

```text
Principle Of Least Privilege
```

Networking has:

> Principle Of Least Connectivity

Allow only necessary communication.

Nothing more.

---

# Example

Bad:

```text
0.0.0.0/0
```

Good:

```text
10.10.20.15

↓

5432

↓

Database
```

Specific.

Restricted.

---

# Threat 7: Blackhole Attacks

Blackhole route:

```text
Destination

↓

Discard packet
```

Sometimes attackers abuse this.

Legitimate uses:

```text
DDoS mitigation

Traffic isolation

Emergency containment
```

---

# Linux Blackhole Example

```bash
sudo ip route add blackhole 10.0.0.0/8
```

Packets disappear.

---

# Threat 8: Rogue Default Gateway

Suppose DHCP is compromised.

Attacker gives:

```text
Gateway:

192.168.1.250
```

Traffic now flows through attacker.

Very dangerous.

---

# Threat 9: DNS + Routing Combination Attacks

Attackers love combining systems.

Attack:

```text
Fake DNS

↓

Fake Gateway

↓

MITM
```

This is why:

```text
HTTPS

TLS

Certificate Validation
```

are critical.

---

# Zero Trust Networking

Old mindset:

```text
Inside network = trusted
```

Wrong.

Modern mindset:

```text
Trust nothing.

Verify everything.
```

---

# Zero Trust Principles

Never trust:

```text
IP

Subnet

Machine

User
```

Always verify.

---

# Zero Trust Architecture

```text
Identity

↓

Authentication

↓

Authorization

↓

Connection
```

Identity first.

Networking second.

---

# Linux Security Tools

Inspect routes:

```bash
ip route
```

Inspect neighbors:

```bash
ip neigh
```

Inspect rules:

```bash
ip rule
```

Inspect interfaces:

```bash
ip addr
```

Inspect firewall:

```bash
iptables -L

nft list ruleset
```

---

# Cloud Security Checklist

Verify:

```text
Subnets

Route Tables

Security Groups

NACLs

Transit Gateway

VPN
```

Always together.

Never individually.

---

# Docker Security Perspective

Bad:

```text
Container

↓

Host Network
```

Excessive privileges.

Good:

```text
Container

↓

Bridge Network
```

Isolated.

---

# Kubernetes Security Perspective

Verify:

```text
Ingress

Services

Network Policies

Pod Security

RBAC
```

Network policies are extremely important.

---

# Secure Packet Flow Example

```text
Internet

↓

WAF

↓

Load Balancer

↓

Application

↓

Database
```

Traffic is controlled at every layer.

---

# Production Security Scenario 1

## Symptom

Database exposed publicly.

Check:

```text
Cloud Route Tables
```

Very common mistake.

---

# Production Security Scenario 2

## Symptom

Pods reachable from internet.

Check:

```text
Ingress

Load Balancers

Services
```

---

# Production Security Scenario 3

## Symptom

Suspicious internal traffic.

Check:

```bash
tcpdump

ip route

ip neigh
```

Investigate lateral movement.

---

# Security Review Workflow

Ask these questions.

Question 1:

Who can talk?

---

Question 2:

Who should not talk?

---

Question 3:

Can traffic be redirected?

---

Question 4:

Can traffic be intercepted?

---

Question 5:

Can attackers move sideways?

---

# Security Decision Tree

```text
New Infrastructure

↓

Public?

↓

Yes

↓

Load Balancer

↓

Application

↓

Database

↓

Can Database Be Reached?

↓

Yes

↓

Fix Immediately

↓

No

↓

Review Segmentation
```

---

# Security Layers

Never rely on one layer.

Use:

```text
Routing

+

Firewall

+

Authentication

+

Encryption

+

Monitoring
```

Defense in depth.

---

# Common Misconceptions

### Misconception 1

"Firewalls solve everything."

Wrong.

Routes matter too.

---

### Misconception 2

"Private IP means secure."

Wrong.

Lateral movement exists.

---

### Misconception 3

"Inside the network is trusted."

Wrong.

Zero Trust says otherwise.

---

### Misconception 4

"Cloud providers secure everything."

Wrong.

You secure your configuration.

---

### Misconception 5

"Kubernetes is secure by default."

Wrong.

Security is your responsibility.

---

# Engineer Mental Model

Never think:

```text
Can server A reach server B?
```

Think:

```text
Should server A reach server B?
```

Huge difference.

Security starts here.

---

# WH Questions

## Why is routing a security topic?

Because traffic movement determines exposure.

---

## Who attacks routing?

Attackers, malware, compromised systems.

---

## When do routing attacks happen?

Continuously.

Internet scanners never stop.

---

## Where do mistakes happen?

Cloud.

Kubernetes.

VPNs.

Linux servers.

---

## How do engineers defend systems?

Segmentation.

Validation.

Monitoring.

Zero Trust.

---

# Key Takeaways

✅ Routing is a security boundary

✅ Traffic is an asset

✅ ARP poisoning is dangerous

✅ Route hijacking is real

✅ BGP hijacking affects the internet

✅ Segmentation is critical

✅ Zero Trust is the future

✅ Cloud route mistakes expose systems

✅ Kubernetes networking requires security thinking

---

# Routing Section Status

Current Files:

```text
networking/Routing/

✅ routing.md

✅ routing-table.md

✅ default-gateway.md

✅ static-routing.md

✅ dynamic-routing-introduction.md

✅ packet-journey.md

✅ internals.md

✅ troubleshooting.md

✅ security.md
```

# Repository Structure Improvement Recommendation

At this point, **I would improve the repository structure**.

Instead of staying inside one huge Routing folder forever, create a dedicated protocol section.

```text
networking/

Routing/

Protocols/
```

Because the next files should become:

```text
networking/Protocols/

routing-protocols-overview.md

rip.md

ospf.md

bgp.md

isis.md

eigrp.md
```

However, **before leaving Routing**, one extremely valuable missing file still remains:

```text
networking/Routing/comparisons.md
```

This file will consolidate all knowledge and massively improve intuition.

It should compare:

```text
Static vs Dynamic

Gateway vs Router

FIB vs RIB

ARP vs Routing

Layer 2 vs Layer 3

Docker vs Kubernetes Networking

Cloud vs Traditional Networking
```

This file will become a high-value reference for engineers.
