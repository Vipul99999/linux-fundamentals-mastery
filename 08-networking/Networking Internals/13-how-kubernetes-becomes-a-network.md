# Story 13: How Kubernetes Becomes A Network ⭐⭐⭐⭐⭐

# Why This File Exists

This is one of the most misunderstood topics in all of infrastructure engineering.

People think:

```text
Kubernetes networking
```

is some giant mysterious technology.

Wrong.

Kubernetes did not invent networking.

Kubernetes orchestrates networking.

This file connects everything you've learned so far.

```text
Networks

↓

Linux

↓

Cloud

↓

Containers

↓

Automation

↓

Kubernetes
```

If you deeply understand this file, Kubernetes networking suddenly becomes much easier.

---

# Learning Goals

After reading this file, you should be able to answer:

- What is Kubernetes networking?
- Why is Kubernetes networking difficult?
- Why does Kubernetes networking exist?
- How do Pods communicate?
- Why do CNIs exist?
- Why do Services exist?
- Why do Ingresses exist?
- Why do engineers think Kubernetes is networking?

---

# The Biggest Misconception

People think:

```text id="8lnqit"
Pod A

↓

Pod B
```

Wrong.

Reality:

```text id="t5q8mz"
Pod

↓

Linux

↓

Linux

↓

Linux

↓

Pod
```

Linux is everywhere.

---

# Kubernetes Is Not A Runtime

This is an extremely important mental model.

Kubernetes is:

> An automation system.

It automates:

```text id="8kns3j"
Scheduling

Networking

Scaling

Healing
```

That's all.

---

# Let's Start Small

Suppose you have:

```text id="bz9o9o"
2 containers
```

Easy.

Visualization:

```text id="8w5jho"
Container A

↓

Container B
```

No problem.

---

# New Problem

Now imagine:

```text id="v2ztvl"
5000 containers
```

Questions appear.

```text id="7y5fj6"
Where are they?

How do they communicate?

How do they move?

How do they scale?
```

Humans invent Kubernetes.

---

# Kubernetes Networking Rule #1 ⭐⭐⭐⭐⭐

Every Pod gets an IP.

This is one of the most important rules.

Wrong:

```text id="bqu2zz"
Node IP
```

Correct:

```text id="vx24ja"
Every Pod has its own IP.
```

---

# Example

Node:

```text id="7b2d0d"
10.0.0.10
```

Pods:

```text id="g1swdv"
10.244.1.2

10.244.1.3

10.244.1.4
```

Pods become first-class network citizens.

---

# Kubernetes Networking Rule #2 ⭐⭐⭐⭐⭐

Pods should communicate without NAT.

This is extremely important.

Pod A:

```text id="r0rzyv"
10.244.1.2
```

should directly reach:

```text id="xwv3g7"
10.244.2.5
```

without special tricks.

---

# Meet Kevin The Cluster

Kevin has one mission.

```text id="7y0k8o"
Make thousands of Linux machines behave like one computer.
```

This is Kubernetes.

---

# The Kubernetes Stack ⭐⭐⭐⭐⭐

Every node contains:

```text id="77ehx6"
Linux

↓

Container Runtime

↓

Pods

↓

Networking Components
```

Visualization:

```text id="zic2di"
Node

↓

Linux

↓

Pods
```

Linux is doing the heavy lifting.

---

# Let's Create A Pod

Question:

Where does networking come from?

Kubernetes asks Linux.

---

# Linux Creates veth Pairs ⭐⭐⭐⭐⭐

Think:

```text id="oq1hzg"
Virtual Ethernet Cable
```

Visualization:

```text id="v4j3lg"
Pod

↓

veth

↓

Linux Host
```

One end goes into the Pod.

One end stays in the node.

---

# Linux Creates A Bridge

Imagine:

```text id="jlwm90"
Pod A

Pod B

Pod C
```

Linux creates:

```text id="mjlwm6"
Bridge
```

Visualization:

```text id="p0m3dt"
Pod

↓

veth

↓

Bridge

↓

Node
```

Bridge behaves like a switch.

---

# But Multiple Nodes Exist

Problem.

```text id="w50x18"
Node A

↓

Node B
```

How do Pods communicate?

Humans invent:

```text id="kzbjlwm"
CNI
```

---

# What Is CNI?

CNI:

```text id="d8nqew"
Container Network Interface
```

Think:

> Networking plugin system for Kubernetes.

Examples:

```text id="4xt89h"
Calico

Cilium

Flannel

Weave
```

---

# CNI Job ⭐⭐⭐⭐⭐

CNI solves:

```text id="v5o45u"
Assign IPs

↓

Create Routes

↓

Connect Nodes

↓

Move Packets
```

That's it.

---

# Pod To Pod Journey ⭐⭐⭐⭐⭐

Suppose:

```text id="mjlwm5"
Pod A

↓

Pod B
```

Different nodes.

Journey:

```text id="kvzyyc"
Pod A

↓

veth

↓

Linux Bridge

↓

Node Router

↓

Cloud Network

↓

Other Node

↓

Linux Bridge

↓

veth

↓

Pod B
```

This diagram is extremely important.

---

# Kubernetes Is Linux Repeated Many Times

This is a huge realization.

Visualization:

```text id="0pr6u7"
Linux

+

Linux

+

Linux

+

Linux

↓

Kubernetes
```

---

# Why Services Exist ⭐⭐⭐⭐⭐

Pods die constantly.

Imagine.

Wrong.

```text id="ptw34r"
Pod IP

↓

Hardcoded
```

Pod dies.

Application breaks.

Humans invent:

```text id="rx4vl5"
Service
```

---

# Service Story

Service gives stable identity.

Visualization:

```text id="cjlwm0"
Client

↓

Service

↓

Pods
```

Pods can change.

Service remains.

---

# kube-proxy Arrives

kube-proxy programs Linux networking.

It creates rules.

Examples:

```text id="8p0lr0"
iptables

IPVS

eBPF
```

depending on implementation.

---

# Ingress Arrives ⭐⭐⭐⭐⭐

Question:

How do users enter cluster?

Humans invent:

```text id="7jlwmv"
Ingress
```

Visualization:

```text id="n8jlwm"
Internet

↓

Ingress

↓

Service

↓

Pods
```

---

# The Entire Kubernetes Flow ⭐⭐⭐⭐⭐

```text id="4jlwm2"
User

↓

Load Balancer

↓

Ingress

↓

Service

↓

Pod

↓

Linux

↓

Pod
```

This is Kubernetes networking.

---

# Kubernetes Doesn't Move Packets

This is extremely important.

Linux moves packets.

Kubernetes configures Linux.

Huge difference.

---

# Cloud Is Also Involved

Cloud providers provide infrastructure.

Example.

```text id="jlwmz8"
EC2

↓

VPC

↓

Subnets

↓

Routes
```

Kubernetes uses them.

---

# The Giant Mental Model ⭐⭐⭐⭐⭐

```text id="jlwmg5"
Linux Networking

+

Cloud Networking

+

Automation

↓

Kubernetes Networking
```

Memorize this.

---

# Why Kubernetes Feels Difficult

Because multiple systems cooperate.

```text id="tjlwm6"
Pods

↓

Linux

↓

CNI

↓

Cloud

↓

Services

↓

Ingress
```

Many layers.

---

# Production Incident Example 1

Symptom:

```text id="jlwm99"
Pods unreachable.
```

Questions:

```text id="jlwm00"
CNI healthy?

Routes healthy?
```

---

# Production Incident Example 2

Symptom:

```text id="jlwm33"
Service unreachable.
```

Questions:

```text id="jlwm44"
kube-proxy healthy?
```

---

# Production Incident Example 3

Symptom:

```text id="jlwm55"
External traffic fails.
```

Questions:

```text id="jlwm66"
Ingress healthy?
```

---

# Production Incident Example 4

Symptom:

```text id="jlwm77"
Cross-node traffic fails.
```

Questions:

```text id="jlwm88"
CNI healthy?

Cloud routes healthy?
```

---

# Universal Kubernetes Algorithm ⭐⭐⭐⭐⭐

Every Kubernetes cluster essentially does this.

```text id="wjlwm7"
Pod

↓

Linux

↓

CNI

↓

Cloud Network

↓

Linux

↓

Pod
```

That's Kubernetes networking.

---

# Engineer Mental Models ⭐⭐⭐⭐⭐

# Mental Model 1

Do not think:

```text id="xjlwm1"
Kubernetes networking
```

Think:

```text id="9jlwm2"
Linux networking automation
```

---

# Mental Model 2

Do not think:

```text id="5jlwm3"
Pods
```

Think:

```text id="0jlwm4"
Processes with networking
```

---

# Mental Model 3

Do not think:

```text id="1jlwm5"
Services
```

Think:

```text id="2jlwm6"
Stable addresses
```

---

# Mental Model 4

Do not think:

```text id="3jlwm7"
Ingress
```

Think:

```text id="4jlwm8"
Cluster entry point
```

---

# Mental Model 5

Do not think:

```text id="5jlwm9"
Kubernetes
```

Think:

```text id="6jlwm0"
Distributed Linux automation
```

---

# Common Misconceptions

### Misconception 1

"Kubernetes invented networking."

Wrong.

Linux did.

---

### Misconception 2

"Pods communicate magically."

Wrong.

Linux networking.

---

### Misconception 3

"CNI is Kubernetes."

Wrong.

CNI is a plugin.

---

### Misconception 4

"Services are proxies."

Partially true.

They are stable abstractions.

---

### Misconception 5

"Kubernetes replaces Linux."

Wrong.

Kubernetes depends on Linux.

---

# WH Questions

## Why does Kubernetes networking exist?

Automation.

---

## Why do Pods get IPs?

Direct communication.

---

## Why do Services exist?

Stable identity.

---

## Why do CNIs exist?

Connect nodes.

---

## How do experts think?

As Linux networking automation.

---

# Key Takeaways

✅ Kubernetes did not invent networking

✅ Linux powers Kubernetes networking

✅ Every Pod gets an IP

✅ CNI connects nodes

✅ Services provide stable identities

✅ Ingress exposes applications

✅ Kubernetes automates distributed Linux networking

---

# Networking Internals Progress ⭐⭐⭐⭐⭐

```text id="7jlwm1"
✓ 01-how-networks-were-born.md

✓ 02-how-a-packet-thinks.md

✓ 03-how-computers-talk.md

✓ 04-how-lan-actually-works.md

✓ 05-how-a-switch-thinks.md

✓ 06-how-a-router-thinks.md

✓ 07-how-a-packet-finds-its-destination.md

✓ 08-how-the-internet-actually-works.md

✓ 09-how-isps-work.md

✓ 10-how-google-receives-your-request.md

✓ 11-how-networks-scale.md

✓ 12-how-cloud-networking-is-built.md

✓ 13-how-kubernetes-becomes-a-network.md
```

# Repository Quality Improvement ⭐⭐⭐⭐⭐

Now we enter the **Operations/SRE section**.

The remaining files are extremely high value.

```text
14-how-packets-get-lost.md ⭐⭐⭐⭐⭐

15-how-network-failures-happen.md ⭐⭐⭐⭐⭐

16-how-engineers-debug-networks.md ⭐⭐⭐⭐⭐

17-how-billion-user-systems-work.md ⭐⭐⭐⭐⭐
```

These files will teach readers how production engineers actually think.
