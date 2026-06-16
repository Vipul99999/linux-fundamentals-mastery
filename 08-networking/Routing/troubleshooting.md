# Routing Troubleshooting

# Why This File Is One Of The Most Important In The Entire Repository

Real engineers do not get paid to memorize commands.

They get paid to answer one question.

> Why is this system not communicating?

Most networking incidents are not:

```text
Hardware failures
```

They are:

```text
Wrong routes

Missing routes

Wrong gateways

Broken DNS

Firewall rules

ARP issues

Policy routing mistakes

VPN issues

Cloud route mistakes

Kubernetes CNI failures
```

Learning commands is easy.

Learning **how to think during incidents** is hard.

This file teaches that mindset.

---

# Learning Goals

After reading this file, you should be able to:

- Build a networking troubleshooting workflow
- Isolate failures quickly
- Identify routing problems
- Debug Linux servers
- Debug Docker networking
- Debug Kubernetes networking
- Debug VPN issues
- Debug cloud networking issues
- Reduce panic during production incidents

---

# The Golden Rule

Never ask:

```text
Why is networking broken?
```

Ask:

```text
Where exactly did the packet stop?
```

This question changes everything.

---

# The Production Engineer Mental Model

Never think:

```text
Server A

↓

Server B
```

Think:

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

NIC

↓

Switch

↓

Router

↓

Internet

↓

Router

↓

Switch

↓

NIC

↓

Application
```

Somewhere in this chain is your problem.

Find it.

---

# The Universal Troubleshooting Framework

Always start with these six questions.

```text
1. Who am I?

2. Where am I trying to go?

3. Do I know how to get there?

4. Can I reach my next hop?

5. Can I see the packet path?

6. Can the response return?
```

This framework solves most incidents.

---

# Step 1: Who Am I?

Always start here.

Never assume.

Check:

```bash
ip addr
```

Example:

```text
eth0:

192.168.1.20/24
```

Questions:

```text
Do I have an IP?

Correct subnet?

Correct interface?
```

---

# Common Failure

No IP.

Symptoms:

```text
No internet

No internal communication

No SSH
```

Possible causes:

```text
DHCP failure

Cable issue

Interface down

VM issue
```

---

# Step 2: Where Am I Going?

Always identify:

```text
Destination IP
```

Never troubleshoot hostnames first.

Bad:

```text
github.com
```

Good:

```text
140.82.121.3
```

Separate:

```text
DNS problem

vs

Routing problem
```

---

# Step 3: Do I Have A Route?

Check:

```bash
ip route
```

Example:

```text
default via 192.168.1.1

192.168.1.0/24 dev eth0
```

Ask:

```text
Does route exist?

Is default route present?

Is specific route present?
```

---

# Step 4: Can I Reach My Gateway?

Check:

```bash
ping gateway-ip
```

Example:

```bash
ping 192.168.1.1
```

If gateway fails:

Problem is nearby.

Not internet.

---

# Step 5: Is ARP Working?

Check:

```bash
ip neigh
```

Good:

```text
192.168.1.1 REACHABLE
```

Bad:

```text
FAILED

INCOMPLETE
```

Potential causes:

```text
Cable issue

VLAN issue

Router issue

ARP poisoning
```

---

# Step 6: Can I See The Packet Path?

Use:

```bash
traceroute destination
```

or

```bash
tracepath destination
```

Example:

```bash
traceroute 8.8.8.8
```

Output:

```text
1 192.168.1.1

2 ISP Router

3 ISP Core

4 Google
```

Packets stopped?

Find the hop.

---

# Step 7: Can Traffic Return?

This is one of the biggest production mistakes.

People only think:

```text
Request path
```

Reality:

```text
Request path

+

Return path
```

Both must work.

---

# The Bidirectional Rule

Networking is always:

```text
A

↓

B

↓

A
```

Never:

```text
A

↓

B
```

---

# Production Incident Workflow

Follow this exact order.

```text
IP

↓

DNS

↓

Route

↓

Gateway

↓

ARP

↓

Traceroute

↓

Firewall

↓

Return Path
```

Memorize this.

---

# Decision Tree (Universal)

```text
Cannot Reach Service

↓

Do I have IP?

↓

No

↓

Fix Interface

↓

Yes

↓

Can DNS Resolve?

↓

No

↓

Fix DNS

↓

Yes

↓

Route Exists?

↓

No

↓

Fix Route

↓

Yes

↓

Gateway Reachable?

↓

No

↓

Fix Layer 2

↓

Yes

↓

Traceroute

↓

Find Broken Hop

↓

Check Firewall

↓

Check Return Path
```

This solves most incidents.

---

# Scenario 1

# No Internet Access

Symptoms:

```text
Cannot open websites

Cannot update packages

Cannot pull Docker images
```

Workflow:

Check IP:

```bash
ip addr
```

Check route:

```bash
ip route
```

Check gateway:

```bash
ping gateway-ip
```

Check internet:

```bash
ping 8.8.8.8
```

Check DNS:

```bash
dig google.com
```

---

# Scenario 2

# Missing Default Route

Problem:

```bash
ip route
```

Output:

```text
192.168.1.0/24 dev eth0
```

Missing:

```text
default route
```

Fix:

```bash
sudo ip route add default via 192.168.1.1
```

---

# Scenario 3

# Wrong Gateway

Configuration:

```text
default via 192.168.2.1
```

Correct:

```text
192.168.1.1
```

Symptoms:

```text
Gateway unreachable
```

Fix route.

---

# Scenario 4

# ARP Failure

Symptoms:

```text
Gateway unreachable
```

Check:

```bash
ip neigh
```

Output:

```text
FAILED
```

Potential causes:

```text
Bad switch

Bad VLAN

Router offline
```

---

# Scenario 5

# DNS Works But Website Fails

DNS:

```bash
dig github.com
```

Works.

Problem:

```text
Routing

Firewall

VPN
```

Investigate further.

---

# Scenario 6

# VPN Connected But Internal Resources Unreachable

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
VPN routes not injected
```

Very common.

---

# Scenario 7

# Split Tunnel Failure

Expected:

```text
Company traffic

↓

VPN

Internet traffic

↓

ISP
```

Reality:

Everything goes through ISP.

Check:

```bash
ip rule

ip route
```

Policy routing issue.

---

# Scenario 8

# Docker Container No Internet

Check:

```bash
docker network ls
```

Check:

```bash
ip route
```

Check:

```bash
iptables -t nat -L
```

Check:

```bash
sysctl net.ipv4.ip_forward
```

Common causes:

```text
NAT broken

IP forwarding disabled

docker0 issue
```

---

# Docker Troubleshooting Flow

```text
Container

↓

docker0

↓

Host Routing

↓

NAT

↓

Gateway

↓

Internet
```

Investigate every layer.

---

# Scenario 9

# Kubernetes Pod Cannot Reach Another Pod

Check:

```bash
kubectl get pods -o wide
```

Check node routes:

```bash
ip route
```

Check CNI:

```bash
kubectl get pods -n kube-system
```

Check:

```text
Calico

Flannel

Cilium
```

Common issue:

Missing Pod routes.

---

# Kubernetes Troubleshooting Flow

```text
Pod

↓

veth

↓

Bridge

↓

Node Route

↓

Other Node

↓

veth

↓

Pod
```

Find broken layer.

---

# Scenario 10

# AWS VPC Communication Failure

Check:

```text
VPC Route Tables

Security Groups

NACLs
```

Very common mistake:

```text
Routes exist

Security Groups block traffic
```

---

# Scenario 11

# VPC Peering Failure

Check:

Both sides.

```text
VPC A

↓

VPC B
```

Both require routes.

Many engineers forget return routes.

---

# Policy Routing Troubleshooting

Check:

```bash
ip rule
```

Example:

```text
0: local

32766: main

32767: default
```

Unexpected rules?

Investigate.

---

# Inspect All Routes

```bash
ip route show table all
```

Useful for:

```text
VPNs

Multi-homed systems

Advanced networking
```

---

# Inspect Interfaces

```bash
ip link
```

Example:

```text
UP

DOWN
```

Simple.

Yet often overlooked.

---

# Inspect Packet Counters

```bash
ip -s link
```

Useful for:

```text
Dropped packets

Errors

Collisions
```

---

# Inspect Traffic Live

```bash
tcpdump
```

Examples:

```bash
sudo tcpdump -i eth0
```

Specific host:

```bash
sudo tcpdump host 8.8.8.8
```

Specific port:

```bash
sudo tcpdump port 443
```

This is one of the most important production tools.

---

# Packet Thinking

Instead of asking:

```text
Why is internet broken?
```

Ask:

```text
Can I see packets?

Where do they stop?
```

Huge mindset shift.

---

# Cloud Engineer Workflow

Never assume cloud is magical.

Cloud networking is:

```text
Routes

↓

Virtual Routers

↓

Virtual Switches

↓

Security Rules
```

Treat it normally.

---

# SRE Mental Model

Every incident becomes:

```text
Observe

↓

Isolate

↓

Narrow Scope

↓

Verify

↓

Fix
```

Never randomly restart things.

---

# Troubleshooting Pyramid

Always troubleshoot from bottom upward.

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

NIC

↓

Physical Network
```

Lower layers first.

---

# Essential Commands Cheat Sheet

Who am I?

```bash
ip addr
```

Routes:

```bash
ip route
```

Gateway:

```bash
ip route show default
```

ARP:

```bash
ip neigh
```

Rules:

```bash
ip rule
```

Interfaces:

```bash
ip link
```

Traceroute:

```bash
traceroute destination
```

Live traffic:

```bash
tcpdump
```

DNS:

```bash
dig domain
```

---

# Production Incident Command Order

Use this exact order.

```bash
ip addr

ip route

ip neigh

ping gateway

traceroute destination

ip rule

tcpdump
```

This order works surprisingly well.

---

# Common Misconceptions

### Misconception 1

"If ping works, networking works."

Wrong.

TCP may still fail.

---

### Misconception 2

"If DNS works, internet works."

Wrong.

Routing may be broken.

---

### Misconception 3

"Cloud networking is different."

Wrong.

Same concepts.

Different implementation.

---

### Misconception 4

"Restarting fixes networking."

Wrong.

Find root cause.

---

### Misconception 5

"One successful ping proves connectivity."

Wrong.

Bidirectional communication matters.

---

# Engineer Mental Model

Never think:

```text
Networking is broken.
```

Think:

```text
Packets are failing somewhere.
```

Your job:

```text
Find where.
```

---

# WH Questions

## Why troubleshoot this way?

Because systems are layered.

---

## Who uses this workflow?

Linux engineers.

DevOps engineers.

SREs.

Cloud engineers.

Backend engineers.

---

## When should we apply it?

Every networking incident.

---

## Where do most failures occur?

Routing.

DNS.

ARP.

Firewalls.

---

## How do experts debug?

By narrowing packet failure points.

---

# Key Takeaways

✅ Always think in packet journeys

✅ Never assume anything

✅ Verify every layer

✅ Troubleshoot from bottom upward

✅ Networking is bidirectional

✅ Docker networking is Linux networking

✅ Kubernetes networking is Linux networking automation

✅ Cloud networking is virtual networking

✅ Find where packets stop

---

# Routing Section Status

Current Repository:

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
```

# Recommended Next File

At this point, the **highest-value missing file** is:

```text
networking/Routing/security.md
```

Reason:

Routing and security are deeply connected.

Topics to cover:

```text
ARP Poisoning

Route Hijacking

BGP Hijacking

Traffic Redirection

Blackhole Routes

VPN Security

Cloud Route Exposure

Segmentation

Zero Trust Networking
```

This will start connecting Linux networking with cybersecurity, cloud security, DevSecOps, and SRE thinking.
