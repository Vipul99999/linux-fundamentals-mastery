# Story 08: How The Internet Actually Works ⭐⭐⭐⭐⭐

# Why This File Exists

Everyone uses the internet.

Very few people understand it.

Ask people:

```text
What is the internet?
```

Common answers:

```text
Google

WiFi

Cloud

5G

The web
```

Wrong.

The internet is none of those things.

The internet is:

> Millions of independent networks agreeing to cooperate.

This file is extremely important because once you understand this, many things become easier.

You'll understand:

- ISPs
- BGP
- Cloud providers
- CDNs
- Data centers
- VPNs
- Multi-region architectures
- Kubernetes networking

much more easily.

---

# Learning Goals

After reading this file, you should be able to answer:

- What is the internet?
- Who owns the internet?
- Why is the internet scalable?
- How do networks cooperate?
- Why do ISPs exist?
- What are autonomous systems?
- Why does BGP exist?
- Why do cloud providers build private networks?

---

# The Biggest Misconception

Most people think:

```text
Laptop

↓

Internet

↓

Google
```

Wrong.

Reality:

```text
Laptop

↓

Home Network

↓

ISP

↓

Transit Networks

↓

Google Network

↓

Google Data Center
```

There is no single "internet box".

---

# The Internet Is Not A Place

The internet is:

> A giant agreement.

Everyone agrees:

```text
I will forward your packets.

You will forward my packets.
```

This cooperation powers the world.

---

# Think Of Countries

Imagine.

Each country builds roads.

```text
India

↓

UAE

↓

UK

↓

USA
```

No single country owns Earth.

Countries cooperate.

The internet works similarly.

---

# The Internet Is Millions Of Networks

Examples:

```text
Google Network

AWS Network

Microsoft Network

Cloudflare Network

Airtel Network

Jio Network

University Networks

Government Networks
```

Millions exist.

---

# Visualization ⭐⭐⭐⭐⭐

```text
Home

↓

ISP

↓

Other ISPs

↓

Cloudflare

↓

Google

↓

AWS

↓

Netflix

↓

Microsoft
```

Everyone cooperates.

---

# Meet Nora The Network

Imagine every company builds its own network.

Google has one.

AWS has one.

Netflix has one.

Universities have one.

These independent networks are called:

```text
Autonomous Systems
```

---

# What Is An Autonomous System?

Autonomous System (AS):

> A network controlled by one organization.

Examples:

```text
Google

Cloudflare

Airtel

AWS

Microsoft
```

Each controls its own infrastructure.

---

# Visualization

```text
Google AS

↓

AWS AS

↓

Cloudflare AS

↓

Airtel AS
```

AS = Network Kingdom.

---

# Why Can't Everyone Connect To Everyone?

Remember Story 01.

Humans always scale.

Direct connections become impossible.

Imagine:

```text
100,000 networks
```

Direct cables?

Impossible.

Humans invented hierarchy.

---

# The Internet Hierarchy ⭐⭐⭐⭐⭐

```text
Your Home

↓

Local ISP

↓

Regional ISP

↓

National ISP

↓

Global Transit Provider

↓

Destination Network
```

This is how packets travel.

---

# Example Journey

You open:

```text
google.com
```

Journey:

```text
Laptop

↓

Home Router

↓

Airtel

↓

Transit Provider

↓

Google Network

↓

Google Server
```

Simple.

Repeated billions of times daily.

---

# The Biggest Internet Secret

Nobody knows the entire internet.

This is extremely important.

Imagine:

```text
10 billion devices
```

Impossible to track everything.

Instead:

```text
Small decisions

↓

Small decisions

↓

Small decisions
```

Scalability emerges.

---

# How Do Networks Know Each Other?

Humans invented:

```text
BGP

Border Gateway Protocol
```

BGP is the language of the internet.

---

# BGP Conversation

Google says:

```text
I own these IPs.
```

Cloudflare says:

```text
I own these IPs.
```

AWS says:

```text
I own these IPs.
```

Everyone learns.

---

# Visualization

```text
Google

↓

BGP

↓

Cloudflare

↓

BGP

↓

Airtel
```

Networks teach each other.

---

# The Internet Is Constantly Changing

Links fail.

Routers fail.

Data centers fail.

BGP adapts.

Packets reroute.

This happens continuously.

---

# Internet Redundancy ⭐⭐⭐⭐⭐

Good infrastructure always has:

```text
Path A

↓

Path B

↓

Path C
```

If one fails:

```text
Traffic reroutes.
```

---

# Undersea Cables Power The Internet

This surprises many people.

Much of the internet is physical.

```text
Fiber Optic Cables

↓

Ocean Floors

↓

Continents
```

Thousands of kilometers.

---

# Visualization

```text
India

↓

Ocean Cable

↓

Middle East

↓

Europe

↓

USA
```

Physics still matters.

---

# The Internet Is Mostly Fiber

Packets travel as:

```text
Light
```

Not magic.

Not wireless.

Mostly light.

---

# Data Centers Are Internet Cities

Google doesn't run one server.

Google runs cities of servers.

Visualization:

```text
Internet

↓

Google Edge

↓

Load Balancer

↓

Thousands Of Servers
```

Massive scale.

---

# CDNs Exist Because Physics Exists

Physics is slow.

India → USA takes time.

Humans invented:

```text
CDN

Content Delivery Network
```

Content moves closer to users.

Examples:

```text
Cloudflare

Akamai

Fastly
```

---

# Why Cloud Providers Build Private Networks

Cloud providers hate the public internet.

Public internet:

```text
Unpredictable
```

Cloud providers build private highways.

Example:

```text
AWS Region

↓

AWS Backbone

↓

Another AWS Region
```

Private infrastructure.

---

# The Public Internet Is Best-Effort

This is a critical concept.

Nobody guarantees:

```text
Fastest route

Shortest route

Cheapest route
```

Networks negotiate continuously.

---

# Why Internet Congestion Happens

Imagine highways.

```text
10 cars

↓

Easy
```

```text
10 million cars

↓

Congestion
```

Networks behave similarly.

---

# Where Production Systems Live

Most large companies use:

```text
Public Internet

+

Private Networks

+

CDNs

+

Load Balancers
```

Everything together.

---

# Example: Opening Google ⭐⭐⭐⭐⭐

```text
Laptop

↓

Home Router

↓

ISP

↓

Transit Provider

↓

Google Edge

↓

Load Balancer

↓

Google Server
```

Everything happens in milliseconds.

---

# Cloud Perspective

AWS.

```text
EC2

↓

VPC Router

↓

AWS Backbone

↓

Internet Gateway

↓

Internet
```

Cloud builds giant private networks.

---

# Kubernetes Perspective

Multi-region Kubernetes.

```text
Cluster A

↓

Cloud Network

↓

Cluster B
```

Still networking.

Just automated.

---

# Why Production Incidents Happen Here

Common issues:

```text
BGP incidents

ISP failures

Undersea cable failures

CDN outages

Cloud outages

Routing mistakes
```

---

# Real Incident Example 1

Symptom:

```text
Website down in India

Works in Europe
```

Possible causes:

```text
ISP issue

CDN issue

BGP issue
```

---

# Real Incident Example 2

Symptom:

```text
AWS region unreachable
```

Possible causes:

```text
Cloud network issue
```

---

# Real Incident Example 3

Symptom:

```text
Slow website globally
```

Possible causes:

```text
No CDN

Congestion

Routing issue
```

---

# Internet Scaling Formula ⭐⭐⭐⭐⭐

This is one of the most important concepts.

```text
Small Networks

↓

BGP

↓

Cooperation

↓

Redundancy

↓

Global Scale
```

This formula built the internet.

---

# Universal Internet Algorithm ⭐⭐⭐⭐⭐

Every packet on Earth essentially follows this.

```text
Local Network

↓

ISP

↓

Transit Network

↓

Destination Network

↓

Destination Server
```

That's the internet.

---

# Engineer Mental Models ⭐⭐⭐⭐⭐

# Mental Model 1

Do not think:

```text
Internet
```

Think:

```text
Millions of cooperating networks
```

---

# Mental Model 2

Do not think:

```text
Google
```

Think:

```text
Google's network
```

---

# Mental Model 3

Do not think:

```text
Cloud
```

Think:

```text
Private global networks
```

---

# Mental Model 4

Do not think:

```text
Internet traffic
```

Think:

```text
Packets crossing many organizations
```

---

# Mental Model 5

Do not think:

```text
Internet = Virtual
```

Think:

```text
Internet = Physical infrastructure + Software decisions
```

---

# Common Misconceptions

### Misconception 1

"The internet is one network."

Wrong.

Millions of networks.

---

### Misconception 2

"Google owns the internet."

Wrong.

Google owns Google's network.

---

### Misconception 3

"WiFi is internet."

Wrong.

WiFi is local access.

---

### Misconception 4

"Cloud replaced the internet."

Wrong.

Cloud uses the internet.

---

### Misconception 5

"Internet is wireless."

Wrong.

Most internet travels through fiber.

---

# WH Questions

## Why does the internet exist?

To connect independent networks.

---

## Who owns the internet?

Nobody.

Everyone.

---

## Why is BGP important?

Networks need a common language.

---

## Why do cloud providers build private networks?

Performance and reliability.

---

## How do experts think?

As networks cooperating together.

---

# Key Takeaways

✅ The internet is a network of networks

✅ Nobody owns the entire internet

✅ ISPs connect users

✅ BGP connects networks

✅ Cloud providers build private backbones

✅ CDNs reduce latency

✅ The internet is physical infrastructure

✅ Production systems depend on internet cooperation

---

# Networking Internals Progress ⭐⭐⭐⭐⭐

```text
✓ 01-how-networks-were-born.md

✓ 02-how-a-packet-thinks.md

✓ 03-how-computers-talk.md

✓ 04-how-lan-actually-works.md

✓ 05-how-a-switch-thinks.md

✓ 06-how-a-router-thinks.md

✓ 07-how-a-packet-finds-its-destination.md

✓ 08-how-the-internet-actually-works.md
```

# Next File ⭐⭐⭐⭐⭐

```text
networking-internals/

09-how-isps-work.md ⭐⭐⭐⭐⭐
```

This is another file that most courses completely skip.

Because every engineer uses ISPs every day but very few understand:

> What exactly does an ISP do after we buy an internet connection?

That knowledge is extremely valuable for infrastructure engineers.
