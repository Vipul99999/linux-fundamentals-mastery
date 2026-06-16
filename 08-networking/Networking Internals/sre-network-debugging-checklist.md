# SRE Network Debugging Checklist ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐

# Why This File Exists

This is not a learning file.

This is a production runbook.

Use this when systems break.

This file teaches one thing:

> Never panic.

Because panicking creates more incidents.

This file gives you a deterministic process.

---

# The Golden Rule ⭐⭐⭐⭐⭐

Never debug systems.

Debug communication.

Everything eventually becomes:

```text
Something

↓

Cannot talk

↓

To something else
```

Examples:

```text
Browser → API

API → Database

Pod → Pod

Pod → Service

VM → VM

Region → Region
```

---

# The Incident Workflow ⭐⭐⭐⭐⭐

Always do this.

```text
Observe

↓

Scope

↓

Gather Evidence

↓

Follow Packet

↓

Find Failure

↓

Fix

↓

Verify

↓

Document
```

Never skip steps.

---

# Phase 0 ⭐⭐⭐⭐⭐

# Stabilize Yourself

Before touching systems.

Do NOT:

```text
Restart random things

Delete Pods

Reboot servers

Modify firewalls

Redeploy everything
```

Stop.

Think.

---

# The Five Golden Questions ⭐⭐⭐⭐⭐

Always answer these first.

Question 1:

```text
What is broken?
```

---

Question 2:

```text
Who is affected?
```

---

Question 3:

```text
When did it start?
```

---

Question 4:

```text
What changed?
```

---

Question 5:

```text
Where did communication stop?
```

Do not proceed until these are answered.

---

# Phase 1 ⭐⭐⭐⭐⭐

# Define The Symptom

Wrong:

```text
Website down
```

Correct:

```text
Users cannot access login API.
```

Wrong:

```text
Kubernetes broken
```

Correct:

```text
Pods cannot communicate across nodes.
```

Specificity wins.

---

# Phase 2 ⭐⭐⭐⭐⭐

# Determine Blast Radius

Question:

```text
Who is affected?
```

---

# Blast Radius Tree

```text
One User?

↓

One Laptop?

↓

One Office?

↓

One Pod?

↓

One Cluster?

↓

One Region?

↓

One Country?

↓

Global?
```

This narrows possibilities enormously.

---

# Clue Matrix ⭐⭐⭐⭐⭐

## One Laptop

Likely:

```text
WiFi

DNS

VPN

Local issue
```

---

## One Office

Likely:

```text
Gateway

ISP

VPN
```

---

## One Cluster

Likely:

```text
Kubernetes

CNI

Cloud routes
```

---

## One Region

Likely:

```text
Cloud

CDN

BGP
```

---

## Global

Likely:

```text
DNS

Cloud outage

Global routing
```

---

# Phase 3 ⭐⭐⭐⭐⭐

# Follow The Packet

Always ask:

```text
Where did Penny stop?
```

The universal path:

```text
Application

↓

DNS

↓

Linux

↓

Gateway

↓

ARP

↓

Switch

↓

Router

↓

ISP

↓

Cloud

↓

Destination

↓

Return Path
```

Somewhere here is the answer.

---

# Phase 4 ⭐⭐⭐⭐⭐

# Check Application Layer

Question:

```text
Was packet created?
```

Check:

```bash
systemctl status nginx

systemctl status docker

systemctl status postgresql
```

Containers:

```bash
docker ps

kubectl get pods
```

---

# Phase 5 ⭐⭐⭐⭐⭐

# Check DNS

Question:

```text
Can names become IPs?
```

Commands:

```bash
dig google.com

dig api.company.com

nslookup api.company.com
```

Failures:

```text
NXDOMAIN

Wrong IP

Timeout
```

---

# Phase 6 ⭐⭐⭐⭐⭐

# Check Linux Networking

Question:

```text
Does Linux know where to send traffic?
```

Commands:

```bash
ip route

ip addr

ip link
```

Healthy example:

```text
default via 192.168.1.1
```

---

# Phase 7 ⭐⭐⭐⭐⭐

# Check Gateway

Question:

```text
Can packet leave home?
```

Commands:

```bash
ping gateway
```

Example:

```bash
ping 192.168.1.1
```

---

# Phase 8 ⭐⭐⭐⭐⭐

# Check ARP

Question:

```text
Can Linux find gateway MAC?
```

Commands:

```bash
ip neigh
```

Healthy:

```text
REACHABLE
```

Unhealthy:

```text
FAILED
```

---

# Phase 9 ⭐⭐⭐⭐⭐

# Check Network Path

Question:

```text
Where did packet stop?
```

Commands:

```bash
traceroute

tracepath
```

Example:

```text
Laptop

↓

Gateway

↓

ISP

↓

Timeout
```

Problem narrowed.

---

# Phase 10 ⭐⭐⭐⭐⭐

# Observe Packets

Question:

```text
Can we see traffic?
```

Commands:

```bash
tcpdump
```

Examples:

```bash
sudo tcpdump -i eth0

sudo tcpdump host api.company.com

sudo tcpdump port 443
```

---

# Phase 11 ⭐⭐⭐⭐⭐

# Observe Connections

Question:

```text
Who is talking?
```

Commands:

```bash
ss -tulpn

ss -tan
```

---

# Phase 12 ⭐⭐⭐⭐⭐

# Check Return Path

This is often forgotten.

Wrong:

```text
A → B
```

Correct:

```text
A → B

↓

B → A
```

Look for:

```text
Firewall

Asymmetric routing

NAT
```

---

# Kubernetes Incident Checklist ⭐⭐⭐⭐⭐

Question 1:

```text
Are Pods alive?
```

```bash
kubectl get pods -o wide
```

---

Question 2:

```text
Are Services healthy?
```

```bash
kubectl get svc
```

---

Question 3:

```text
Are Nodes healthy?
```

```bash
kubectl get nodes
```

---

Question 4:

```text
Is CNI healthy?
```

---

Question 5:

```text
Is Ingress healthy?
```

---

# Cloud Incident Checklist ⭐⭐⭐⭐⭐

Question:

```text
Subnet?

↓

Route Table?

↓

Security Group?

↓

NACL?

↓

IGW?

↓

NAT Gateway?
```

Most cloud incidents live here.

---

# Load Balancer Checklist ⭐⭐⭐⭐⭐

Questions:

```text
DNS correct?

↓

Health checks passing?

↓

Backend healthy?

↓

Certificates valid?
```

---

# VPN Checklist ⭐⭐⭐⭐⭐

Questions:

```text
VPN connected?

↓

Routes installed?

↓

DNS updated?

↓

Default route correct?
```

Commands:

```bash
ip route

resolvectl status
```

---

# Database Checklist ⭐⭐⭐⭐⭐

Questions:

```text
Database alive?

↓

Can application reach it?

↓

Firewall healthy?

↓

Connection exhaustion?
```

---

# API Latency Checklist ⭐⭐⭐⭐⭐

Questions:

```text
CPU?

↓

Memory?

↓

Database?

↓

Retries?

↓

Congestion?
```

Observe:

```text
Latency

Errors

Traffic

Saturation
```

---

# The Four Golden Signals ⭐⭐⭐⭐⭐

Always observe these.

```text
Latency

Traffic

Errors

Saturation
```

These reveal hidden failures.

---

# Timeline Analysis ⭐⭐⭐⭐⭐

Always ask:

```text
What happened before this?
```

Timeline:

```text
Healthy

↓

Deployment

↓

Traffic Spike

↓

Failures

↓

Incident
```

Time is evidence.

---

# Change Correlation ⭐⭐⭐⭐⭐

Always ask:

```text
What changed?
```

Examples:

```text
Firewall

DNS

Deployments

Cloud changes

Certificates
```

Many incidents start here.

---

# The Top 20 Failure Zones ⭐⭐⭐⭐⭐

Most incidents repeat here.

```text
DNS

Routes

ARP

Gateway

Firewalls

NAT

ISP

BGP

CDN

Load Balancers

Caches

Databases

Retries

Congestion

Cloud Routes

Security Groups

CNI

kube-proxy

Ingress

Humans 😄
```

Memorize these.

---

# The Universal Incident Flowchart ⭐⭐⭐⭐⭐

Every incident eventually becomes:

```text
Alert

↓

Symptom

↓

Scope

↓

Packet Journey

↓

Dependency Graph

↓

Root Cause

↓

Fix

↓

Verify

↓

Postmortem
```

---

# The Dependency Rule ⭐⭐⭐⭐⭐

Always ask:

```text
What does this system depend on?
```

Example:

```text
Frontend

↓

API

↓

Auth

↓

Cache

↓

Database

↓

Storage
```

Dependencies hide failures.

---

# The Senior Engineer Secret ⭐⭐⭐⭐⭐

Senior engineers are not smarter.

They simply remove possibilities faster.

Huge difference.

---

# The Elimination Tree ⭐⭐⭐⭐⭐

```text
Website Down

↓

DNS Healthy?

↓

Linux Healthy?

↓

Gateway Healthy?

↓

Route Healthy?

↓

Cloud Healthy?

↓

Destination Healthy?
```

Remove possibilities.

Never guess.

---

# Engineer Mental Models ⭐⭐⭐⭐⭐

# Mental Model 1

Do not think:

```text
Website Down
```

Think:

```text
Communication Stopped
```

---

# Mental Model 2

Do not think:

```text
Find Answer
```

Think:

```text
Eliminate Possibilities
```

---

# Mental Model 3

Do not think:

```text
Commands
```

Think:

```text
Questions
```

---

# Mental Model 4

Do not think:

```text
Systems
```

Think:

```text
Packet Journeys
```

---

# Mental Model 5

Do not think:

```text
Outages
```

Think:

```text
Conversations That Broke
```

---

# The Ultimate SRE Equation ⭐⭐⭐⭐⭐

```text
Observe

↓

Scope

↓

Evidence

↓

Packet Journey

↓

Failure Point

↓

Fix

↓

Verify

↓

Learn
```

Memorize this.

This is how SREs keep civilization online.

---

# 🚨 Print This Section ⭐⭐⭐⭐⭐

If you only remember one thing from this file, remember this:

```text
1. What broke?

2. Who is affected?

3. What changed?

4. Where did communication stop?

5. Fix only after evidence.
```

That alone will solve an enormous percentage of production incidents.

---

# Repository Cross Links ⭐⭐⭐⭐⭐

This file should be linked from:

```text
networking-internals/

14-how-packets-get-lost.md

15-how-network-failures-happen.md

16-how-engineers-debug-networks.md

packet-debugging-playbook.md

production-incidents.md

packet-journey-cheatsheet.md
```

because these files together become your **Production Networking Operating System**.
