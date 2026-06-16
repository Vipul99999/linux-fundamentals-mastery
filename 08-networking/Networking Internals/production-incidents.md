# Production Incidents: How Real Systems Break ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐

# Why This File Exists

This may be one of the highest ROI files in the repository.

Most engineers learn technologies.

Very few engineers learn incidents.

Production engineering is mostly pattern recognition.

After enough incidents, engineers start seeing the same patterns repeatedly.

This file accelerates that learning.

The goal:

```text
Symptoms

↓

Clues

↓

Packet Journey

↓

Root Cause

↓

Recovery
```

This is how senior engineers think.

---

# The Golden Rule ⭐⭐⭐⭐⭐

Incidents rarely happen because of one thing.

Most outages look like this.

```text
Small Failure

↓

Hidden Dependency

↓

Retries

↓

Congestion

↓

Cascade Failure

↓

Outage
```

Memorize this.

---

# Incident 1 ⭐⭐⭐⭐⭐

# Website Down But Server Is Healthy

# Symptom

```text
Users cannot access website.

Server is healthy.
```

---

# Junior Engineer Thought

```text
Server problem.
```

Wrong.

---

# Senior Engineer Thought

```text
Communication problem.
```

---

# Packet Journey

```text
Browser

↓

DNS

↓

Gateway

↓

ISP

↓

Cloud

↓

Load Balancer

↓

Server
```

Packet stopped somewhere.

---

# Likely Root Causes

```text
DNS issue

Load balancer issue

Firewall issue

CDN issue
```

---

# Questions

```text
Can DNS resolve?

Can users reach load balancer?

Can load balancer reach server?
```

---

# Incident 2 ⭐⭐⭐⭐⭐

# Website Works For Europe But Not India

# Symptom

```text
India users affected.

Europe users healthy.
```

---

# Think Geographically

Question:

```text
What only India users use?
```

Examples:

```text
ISP

CDN edge

BGP path
```

---

# Packet Journey

```text
India

↓

ISP

↓

CDN Edge

↓

Cloud
```

---

# Likely Causes

```text
ISP outage

BGP issue

CDN issue
```

---

# Incident 3 ⭐⭐⭐⭐⭐

# Kubernetes Pods Cannot Talk To Each Other

# Symptom

```text
Pod A

↓

Pod B

×

Fail
```

---

# Junior Engineer

```text
Kubernetes broken.
```

Wrong.

---

# Senior Engineer

```text
Which networking layer failed?
```

---

# Packet Journey

```text
Pod

↓

veth

↓

Bridge

↓

Node

↓

CNI

↓

Cloud Network

↓

Other Node

↓

Pod
```

---

# Likely Causes

```text
CNI issue

Routes missing

Firewall issue
```

---

# Checklist

```text
kubectl get nodes

kubectl get pods -o wide

kubectl get svc

kubectl logs
```

---

# Incident 4 ⭐⭐⭐⭐⭐

# API Suddenly Slow

# Symptom

```text
API latency increased.

No errors.
```

---

# Junior Engineer

```text
Optimize code.
```

Wrong.

---

# Senior Engineer

```text
Find bottleneck.
```

---

# Dependency Graph

```text
API

↓

Authentication

↓

Cache

↓

Database

↓

Third Party API
```

---

# Possible Causes

```text
Database latency

Retry storm

Congestion

Slow dependencies
```

---

# Incident 5 ⭐⭐⭐⭐⭐

# VPN Connected But Nothing Works

# Symptom

```text
VPN connected.

Resources unreachable.
```

---

# Most Common Cause

Routing.

---

# Packet Journey

```text
Laptop

↓

VPN Interface

↓

VPN Gateway

↓

Company Network
```

---

# Questions

```text
Did VPN install routes?

Did DNS change?

Did default route change?
```

---

# Check

```bash
ip route

resolvectl status
```

---

# Incident 6 ⭐⭐⭐⭐⭐

# Database Healthy But Application Times Out

# Symptom

```text
Database up.

Application timing out.
```

---

# Think Dependency Chain

```text
Application

↓

Firewall

↓

Network

↓

Database
```

---

# Possible Causes

```text
Security group

Firewall

NAT issue

Connection exhaustion
```

---

# Incident 7 ⭐⭐⭐⭐⭐

# CPU Suddenly 100%

# Symptom

```text
Everything slow.
```

---

# Common Hidden Causes

```text
Retry storm

Traffic spike

Infinite loop

Log explosion
```

---

# Packet Thinking

```text
More Requests

↓

More CPU

↓

Slower Responses

↓

Retries

↓

More Requests
```

Feedback loop.

---

# Incident 8 ⭐⭐⭐⭐⭐

# DNS Outage

# Symptom

```text
Nothing works.
```

---

# Reality

DNS is often a giant hidden dependency.

Visualization:

```text
Users

↓

DNS

💥

Everything stops
```

---

# Incident 9 ⭐⭐⭐⭐⭐

# Load Balancer Healthy But Users Cannot Connect

# Symptom

```text
Load balancer healthy.

Users affected.
```

---

# Likely Causes

```text
DNS

BGP

CDN

ISP
```

Always think upstream.

---

# Incident 10 ⭐⭐⭐⭐⭐

# Database Connection Exhaustion

# Symptom

```text
Too many connections.
```

---

# Root Cause Pattern

```text
Traffic Spike

↓

More Requests

↓

More Connections

↓

Limit Reached
```

---

# Solutions

```text
Pooling

Caching

Queues
```

---

# Incident 11 ⭐⭐⭐⭐⭐

# NAT Port Exhaustion

# Symptom

```text
Outbound internet randomly fails.
```

Very common in cloud systems.

---

# Packet Journey

```text
Private Subnet

↓

NAT Gateway

↓

Internet
```

Too many connections.

Ports exhausted.

---

# Incident 12 ⭐⭐⭐⭐⭐

# One Kubernetes Node Broken

# Symptom

```text
Some Pods healthy.

Some Pods unreachable.
```

---

# Think Node Specific

Possible causes:

```text
Node network

CNI issue

Bridge issue
```

---

# Incident 13 ⭐⭐⭐⭐⭐

# Cloud Region Outage

# Symptom

```text
Entire region affected.
```

---

# Good Architecture

```text
Mumbai

↓

Singapore

↓

Tokyo
```

Failover exists.

---

# Bad Architecture

```text
Mumbai Only

💥

Everything Down
```

---

# Incident 14 ⭐⭐⭐⭐⭐

# Thundering Herd Problem

# Symptom

```text
Service recovers.

Immediately crashes again.
```

---

# Pattern

```text
1 Million Users

↓

Reconnect

↓

Overload

↓

Crash Again
```

---

# Solution

```text
Jitter

Backoff

Rate limiting
```

---

# Incident 15 ⭐⭐⭐⭐⭐

# Cascading Failure

# Symptom

```text
Everything slowly dies.
```

---

# Pattern

```text
Database Slow

↓

API Slow

↓

Retries

↓

Congestion

↓

Outage
```

This is extremely common.

---

# Incident 16 ⭐⭐⭐⭐⭐

# Human Error

# Symptom

```text
Everything suddenly broke.
```

Question:

```text
What changed?
```

Common examples:

```text
Firewall rules

DNS changes

Deployments

Routes
```

---

# The Universal Incident Flowchart ⭐⭐⭐⭐⭐

Every incident eventually becomes this.

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

# The Five Golden Questions ⭐⭐⭐⭐⭐

Always ask these.

Question 1:

```text
What is broken?
```

Question 2:

```text
Who is affected?
```

Question 3:

```text
When did it start?
```

Question 4:

```text
What changed?
```

Question 5:

```text
Where did communication stop?
```

These solve countless incidents.

---

# The Top 20 Production Failure Zones ⭐⭐⭐⭐⭐

Engineers repeatedly find incidents here.

```text
DNS

Linux Routes

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

Humankind 😄
```

---

# The Incident Pyramid ⭐⭐⭐⭐⭐

```text
Tiny Misconfiguration

↓

Hidden Dependency

↓

Traffic Spike

↓

Retries

↓

Congestion

↓

Outage
```

Most outages follow this.

---

# The Incident Recovery Pyramid ⭐⭐⭐⭐⭐

```text
Detect

↓

Scope

↓

Isolate

↓

Reroute

↓

Recover

↓

Verify

↓

Learn
```

This is SRE thinking.

---

# The Four Universal Incident Categories ⭐⭐⭐⭐⭐

Every incident eventually falls into one.

```text
Communication

Scale

Failure

Human Error
```

Memorize these.

---

# The Ultimate Engineer Mental Model ⭐⭐⭐⭐⭐

Do not think:

```text
Website Down
```

Think:

```text
A Conversation Somewhere Stopped Working
```

That's production engineering.

---

# Key Takeaways

✅ Incidents repeat patterns

✅ Follow packets

✅ Follow dependencies

✅ Follow geography

✅ Follow timelines

✅ Follow recent changes

✅ Communication is the foundation

---

# Repository Recommendation ⭐⭐⭐⭐⭐

This file should be heavily cross-linked with:

```text
networking-internals/

14-how-packets-get-lost.md

15-how-network-failures-happen.md

16-how-engineers-debug-networks.md

packet-debugging-playbook.md
```

These four files together become your **Production Engineering Core**.
