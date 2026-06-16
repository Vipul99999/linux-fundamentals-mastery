# Dynamic Routing (Introduction)

# Why This Matters

Static routing is excellent...

...until infrastructure becomes large.

Imagine manually managing routes for:

- 10 Linux servers
- 100 Kubernetes nodes
- 500 branch offices
- 2000 cloud networks
- Thousands of routers
- Multiple data centers
- Multi-region infrastructure

Very quickly, static routing becomes impossible.

Infrastructure engineers solved this problem by allowing routers to talk to each other.

Instead of humans manually teaching routes, routers learn routes automatically.

This is:

> Dynamic Routing

Dynamic routing is one of the foundational technologies that makes the modern internet possible.

Without it:

- Internet would not scale
- Large enterprises would not scale
- ISPs would not exist
- Multi-data center infrastructure would be difficult
- Global cloud providers would be impossible

---

# Learning Goals

After reading this file, you should be able to answer:

- What is dynamic routing?
- Why was it invented?
- What problem does it solve?
- How is it different from static routing?
- How do routers learn routes?
- What are routing protocols?
- What is route advertisement?
- What is convergence?
- Why does the internet need BGP?
- When do engineers use dynamic routing?

---

# Start With A Problem

Suppose your company has three offices.

```text
Delhi

↓

Mumbai

↓

Bangalore
```

Networks:

```text
Delhi

10.1.0.0/16

Mumbai

10.2.0.0/16

Bangalore

10.3.0.0/16
```

With static routes:

Every router must manually know every network.

Very quickly this becomes painful.

---

# Static Routing Explosion

Router Delhi:

```text
10.2.0.0/16

10.3.0.0/16
```

Router Mumbai:

```text
10.1.0.0/16

10.3.0.0/16
```

Router Bangalore:

```text
10.1.0.0/16

10.2.0.0/16
```

Add 100 offices.

The problem explodes.

---

# The Big Idea

Instead of humans teaching routes:

Routers teach each other.

```text
Router A

↓

Router B

↓

Router C
```

They exchange information.

This information sharing is called:

> Route Advertisement

---

# What Is Dynamic Routing?

Dynamic routing is:

> The automatic exchange and maintenance of routing information between routers.

Routers continuously learn.

Routers continuously update.

Routers continuously adapt.

---

# Think Like GPS Navigation

Static routing:

```text
Printed paper map.
```

Dynamic routing:

```text
Google Maps with live traffic updates.
```

Road closed?

Google recalculates.

Link fails?

Routers recalculate.

Exactly the same idea.

---

# Dynamic Routing Solves Three Big Problems

## Problem 1

Scalability

Without dynamic routing:

```text
Human work

↑↑↑↑↑
```

Infrastructure grows.

Dynamic routing:

```text
Automation

↑

Infrastructure grows
```

Human work stays manageable.

---

## Problem 2

Failure Recovery

Suppose:

```text
Router B dies
```

Static routing:

```text
Everything breaks.
```

Dynamic routing:

```text
Routers recalculate paths.
```

---

## Problem 3

Adaptability

Infrastructure changes constantly.

Examples:

- Servers added
- Routers removed
- New data centers
- New VPNs
- New cloud regions

Dynamic routing adapts.

---

# High Level Architecture

```text
Router A

↓

Router B

↓

Router C
```

Routers exchange:

```text
Networks

Metrics

Costs

Reachability
```

Each router builds its own routing table.

---

# Dynamic Routing Workflow

```text
Router Starts

↓

Discovers Neighbors

↓

Exchange Routes

↓

Build Topology

↓

Calculate Best Paths

↓

Install Routes

↓

Monitor Changes

↓

Update Continuously
```

This loop never stops.

---

# Dynamic Routing Components

There are several moving parts.

## Neighbors

Routers discover each other.

---

## Advertisements

Routers share networks.

---

## Metrics

Routers compare paths.

---

## Topology Database

Routers build a network map.

---

## Convergence

Routers agree on the network state.

---

# Route Advertisement

Suppose:

Router A knows:

```text
10.1.0.0/16
```

Router B knows:

```text
10.2.0.0/16
```

Router A says:

```text
I can reach:

10.1.0.0/16
```

Router B learns it.

Now Router B says:

```text
I can reach:

10.2.0.0/16
```

Router A learns it.

---

# Visual Example

```text
Router A

10.1.0.0/16

↓

Advertisement

↓

Router B

10.2.0.0/16
```

Both learn.

Both update.

---

# Dynamic Routing Is Continuous

This is important.

Dynamic routing is not:

```text
Configure once

Done forever
```

Instead:

```text
Learn

Monitor

Update

Repeat
```

Forever.

---

# The Concept Of Convergence

Convergence means:

> Every router eventually agrees on the network topology.

Imagine:

```text
Router A

↓

Router B

↓

Router C
```

Router B fails.

Routers need time to realize:

```text
B is gone
```

Then:

```text
Find alternative path
```

Once everyone agrees:

```text
Converged
```

---

# Slow Convergence Is Dangerous

During convergence:

Packets may:

```text
Loop

↓

Drop

↓

Take wrong paths
```

Fast convergence is extremely important.

Especially for:

- Financial systems
- Banking
- Kubernetes clusters
- Cloud infrastructure

---

# What Are Routing Protocols?

Dynamic routing requires languages.

Routers must speak the same language.

These languages are:

> Routing Protocols

Examples:

```text
RIP

OSPF

EIGRP

IS-IS

BGP
```

---

# Think Of Protocols Like Human Languages

```text
English

Hindi

Japanese
```

Routers:

```text
OSPF

BGP

RIP
```

If routers speak different protocols:

Communication fails.

---

# Major Dynamic Routing Protocols

| Protocol | Purpose |
|----------|---------|
| RIP | Small networks |
| OSPF | Enterprises |
| IS-IS | Service providers |
| EIGRP | Cisco environments |
| BGP | Internet |

---

# RIP

Routing Information Protocol.

Old.

Simple.

Uses:

```text
Hop count
```

Rarely used today.

---

# OSPF

Open Shortest Path First.

Extremely common.

Enterprise favorite.

Fast.

Reliable.

Large scale.

---

# IS-IS

Popular with:

- ISPs
- Telecom providers

Very scalable.

---

# BGP

Border Gateway Protocol.

This is huge.

BGP powers:

> The Internet

Google.

AWS.

Microsoft.

Cloudflare.

Netflix.

ISPs.

Everyone uses BGP.

---

# Interior vs Exterior Routing

There are two worlds.

---

## Interior Routing

Inside one organization.

Examples:

```text
Company network

Data center

Campus network
```

Protocols:

```text
OSPF

IS-IS

EIGRP
```

---

## Exterior Routing

Between organizations.

Examples:

```text
Google

↓

Cloudflare

↓

AWS

↓

ISP
```

Protocol:

```text
BGP
```

---

# Linux And Dynamic Routing

Linux itself supports dynamic routing.

Usually through routing daemons.

Examples:

```text
FRRouting

Bird

Quagga
```

Linux becomes a router.

---

# Example

Install FRRouting.

```text
Linux

↓

FRR

↓

OSPF/BGP

↓

Enterprise Router
```

Very common.

---

# Docker Usually Doesn't Need Dynamic Routing

Docker networks are relatively small.

Static routes are enough.

---

# Kubernetes Is Moving Toward Dynamic Routing

Small clusters:

```text
Static routes
```

Large clusters:

```text
BGP
```

Examples:

```text
Calico + BGP
```

Nodes advertise Pod networks.

---

# Kubernetes Example

Node A:

```text
10.244.1.0/24
```

Node B:

```text
10.244.2.0/24
```

Calico advertises:

```text
I own:

10.244.1.0/24
```

Other nodes learn automatically.

No manual routes.

---

# Cloud Example

Cloud providers hide enormous routing complexity.

AWS internally uses:

- Dynamic routing
- Software-defined networking

Services involved:

```text
Transit Gateway

Direct Connect

VPN Gateway
```

Many use BGP.

---

# Hybrid Cloud Example

```text
Office

↓

VPN

↓

AWS

↓

Azure
```

BGP exchanges routes.

Everything updates automatically.

---

# Linux Internals Perspective

Dynamic routing software does NOT directly forward packets.

Instead:

```text
Routing Daemon

↓

Netlink Socket

↓

Kernel FIB

↓

Linux Routing Table
```

Packets still use:

```text
FIB
```

Dynamic routing simply updates it automatically.

---

# Static vs Dynamic Routing

| Feature | Static | Dynamic |
|---------|--------|---------|
| Manual configuration | Yes | No |
| Automatic updates | No | Yes |
| Learns failures | No | Yes |
| Scales well | No | Excellent |
| Complexity | Low | Higher |
| CPU usage | Low | Moderate |
| Maintenance effort | High | Lower |

---

# Production Scenario 1

## Symptom

Branch office cannot reach data center.

Static routes exist everywhere.

New office added.

Nobody updated routes.

Dynamic routing would solve this.

---

# Production Scenario 2

## Symptom

ISP router fails.

Dynamic routing:

```text
Detect failure

↓

Reroute traffic

↓

Recover
```

Users may not notice.

---

# Production Scenario 3

## Symptom

Large Kubernetes cluster becomes unmanageable.

Thousands of Pod routes.

Solution:

```text
Calico + BGP
```

---

# Troubleshooting Mental Model

Always ask:

Question 1:

Did routers become neighbors?

---

Question 2:

Did routes get advertised?

---

Question 3:

Did routes enter the routing table?

```bash
ip route
```

---

Question 4:

Did convergence complete?

---

Question 5:

Can packets reach destination?

```bash
traceroute
```

---

# Dynamic Routing Decision Flow

```text
Network Problem

↓

Neighbor Established?

↓

No

↓

Fix Connectivity

↓

Yes

↓

Routes Learned?

↓

No

↓

Check Advertisements

↓

Yes

↓

Converged?

↓

No

↓

Wait Or Investigate

↓

Yes

↓

Test Traffic
```

---

# Security Considerations

Dynamic routing is powerful.

But dangerous if unsecured.

Risks:

```text
Fake routes

↓

Traffic hijacking

↓

Traffic blackholing

↓

Man-in-the-middle attacks
```

Protect using:

```text
Authentication

Filtering

Access Control

Route Validation
```

---

# Common Misconceptions

### Misconception 1

"Dynamic routing is faster."

Wrong.

Packet forwarding speed is unchanged.

Only route management changes.

---

### Misconception 2

"Dynamic routing replaces Linux routing tables."

Wrong.

It updates them.

---

### Misconception 3

"Only hardware routers support dynamic routing."

Wrong.

Linux supports it.

---

### Misconception 4

"BGP is only for ISPs."

Wrong.

Many enterprises use it too.

---

# Engineer Mental Model

Do not think:

```text
Routers know everything.
```

Think:

```text
Routers continuously learn everything.
```

Infrastructure is a living system.

Dynamic routing allows it to adapt.

---

# WH Questions

## Why was dynamic routing invented?

Static routing doesn't scale.

---

## Who uses it?

ISPs.

Cloud providers.

Large enterprises.

Kubernetes environments.

---

## When should engineers use it?

Large and changing infrastructures.

---

## Where does it run?

Routers and Linux routing daemons.

---

## How does it work?

Routers exchange routing information continuously.

---

# Key Takeaways

✅ Dynamic routing automates route management

✅ Routers learn routes from each other

✅ Dynamic routing solves scalability problems

✅ Convergence is critical

✅ Routing protocols power dynamic routing

✅ BGP powers the internet

✅ Linux supports dynamic routing

✅ Kubernetes increasingly uses BGP

✅ Cloud infrastructure heavily depends on dynamic routing

---

# Routing Section Status

Core files completed:

```text
networking/Routing/

✅ routing.md

✅ routing-table.md

✅ default-gateway.md

✅ static-routing.md

✅ dynamic-routing-introduction.md
```

# Repository Expansion Recommendation

At this point, this section is large enough that the following supporting files should now become first-class citizens.

```text
networking/Routing/

routing.md

routing-table.md

default-gateway.md

static-routing.md

dynamic-routing-introduction.md

internals.md

packet-journey.md

troubleshooting.md

security.md

visuals.md

comparisons.md

labs.md
```

# Recommended Next File (IMPORTANT)

Instead of moving away from Routing, we should now create:

```text
networking/Routing/packet-journey.md
```

Reason:

The audience now knows individual pieces.

Before introducing OSPF/BGP deeply, we should build a strong engineer mental model by showing:

```text
Application

↓

Socket

↓

Kernel

↓

FIB

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
```

This file will dramatically improve troubleshooting capability for:

- Linux engineers
- Backend engineers
- DevOps engineers
- SREs
- Cloud engineers
