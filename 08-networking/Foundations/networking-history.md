# Networking History

> Understanding the evolution of networking helps you understand why modern Linux networking is designed the way it is today.

---

# Why Learn Networking History?

Many beginners skip networking history.

This is a mistake.

History explains:

- Why protocols exist
- Why the internet is designed this way
- Why layers exist
- Why IP addresses exist
- Why TCP exists
- Why DNS exists
- Why Linux became dominant

If you understand history, modern networking becomes easier.

---

# Before Networking Existed

Computers were isolated machines.

```text
Computer A

❌

Computer B
```

Problems:

- Cannot share files
- Cannot communicate
- Cannot share resources
- Cannot share printers
- Cannot collaborate

Every computer was an island.

---

# The First Problem: Resource Sharing

Computers were expensive.

Organizations asked:

> Why buy a printer for every computer?

They wanted:

```text
Computer A

↓

Shared Resource

↑

Computer B
```

Networking was born from resource sharing.

---

# 1950s - Standalone Mainframes

Architecture:

```text
Users

↓

Terminals

↓

Mainframe Computer
```

Characteristics:

- Very expensive
- Centralized computing
- No internet
- No global communication

---

# 1960s - Terminal Era

Multiple users connected to one machine.

```text
User

↓

Terminal

↓

Mainframe

↑

Terminal

↑

User
```

Still no computer-to-computer communication.

---

# The Big Question

Researchers asked:

> Can computers talk to each other?

This question changed the world.

---

# 1969 - ARPANET

ARPANET was created.

Goal:

```text
Connect universities and research centers.
```

Original nodes:

```text
UCLA

↓

Stanford

↓

UCSB

↓

Utah
```

---

# Why Was ARPANET Revolutionary?

Before:

```text
1 Computer

↓

1 User
```

After:

```text
Computer

↓

Network

↓

Computer
```

Machines could communicate.

---

# ARPANET's Biggest Idea

Decentralization.

Instead of:

```text
One giant computer
```

Use:

```text
Many independent computers
```

---

# Centralized System Problem

```text
Clients

↓

Main Server
```

If server dies:

```text
Everything dies
```

Single point of failure.

---

# Decentralized Network

```text
Node

↔

Node

↔

Node

↔

Node
```

If one fails:

```text
Others continue
```

This principle still powers the internet.

---

# Packet Switching Was Invented

Old communication:

```text
Dedicated path

Source

========

Destination
```

Problem:

- Expensive
- Inefficient

---

# Packet Switching

Data became small packets.

```text
Large Data

↓

Packet 1

Packet 2

Packet 3

Packet 4
```

Packets could travel independently.

This changed everything.

---

# Why Packet Switching Won

Advantages:

```text
Efficient

Flexible

Fault tolerant

Scalable
```

Modern internet still uses it.

---

# 1970s Problem

Different networks appeared.

Example:

```text
Military Network

University Network

Research Network
```

Problem:

```text
They couldn't communicate.
```

A universal language was needed.

---

# TCP/IP Was Born

Researchers created:

```text
TCP/IP
```

Goal:

> Any network can communicate with any other network.

---

# Why TCP/IP Was Revolutionary

Before:

```text
Network A

❌

Network B
```

After:

```text
Network A

↓

TCP/IP

↓

Network B
```

Everything could interconnect.

This created:

```text
Inter-Network

↓

Internet
```

---

# 1983 - TCP/IP Officially Adopted

ARPANET switched to TCP/IP.

Many engineers call this:

> The birth of the modern internet.

---

# Domain Names Did Not Exist

Early users had to remember:

```text
192.168.0.15

8.8.8.8

31.13.71.36
```

Problem:

Humans are bad at remembering numbers.

---

# DNS Was Invented

DNS translates:

```text
google.com

↓

142.251.x.x
```

DNS became the internet phonebook.

---

# OSI Model Was Created

Problem:

Everyone built networking differently.

Solution:

Standardization.

OSI introduced layers.

```text
Application

Presentation

Session

Transport

Network

Data Link

Physical
```

Even though Linux mainly uses TCP/IP, OSI is still important for learning.

---

# Ethernet Became Popular

Ethernet solved local communication.

Devices could connect inside buildings.

```text
Computer

↓

Switch

↓

Computer
```

Ethernet dominates LANs today.

---

# Routers Emerged

Networks grew.

A new problem appeared.

```text
How do packets find destinations?
```

Routers solved this.

```text
Network A

↓

Router

↓

Network B
```

---

# 1990s - World Wide Web

Tim Berners-Lee created:

```text
HTTP

HTML

URLs
```

The web was born.

Users could browse websites.

---

# Simple Web Flow

```text
Browser

↓

HTTP Request

↓

Server

↓

HTML

↓

Browser
```

This is still happening today.

---

# Linux Enters The Story

Linux was released in 1991.

Why Linux dominated networking:

```text
Open Source

Stable

Fast

Flexible

Secure

Scalable
```

---

# Linux Powers The Internet

Today Linux powers:

```text
Cloud Servers

Web Servers

Datacenters

Supercomputers

Kubernetes

Docker

AI Infrastructure

IoT Devices
```

---

# Modern Internet Architecture

```text
User

↓

DNS

↓

CDN

↓

Load Balancer

↓

Application Server

↓

Microservices

↓

Database
```

Linux exists almost everywhere.

---

# Modern Networking Timeline

```text
1950s

↓

Standalone Mainframes

↓

1960s

↓

Terminals

↓

1969

↓

ARPANET

↓

1970s

↓

Packet Switching

↓

TCP/IP Development

↓

1983

↓

TCP/IP Adoption

↓

1984

↓

DNS

↓

1990

↓

World Wide Web

↓

1991

↓

Linux

↓

2000s

↓

Cloud Computing

↓

2010s

↓

Containers

↓

2014+

↓

Kubernetes

↓

Today

↓

Cloud Native Infrastructure
```

---

# Evolution Of Infrastructure

## Era 1

```text
One Computer

↓

One User
```

---

## Era 2

```text
Many Users

↓

One Mainframe
```

---

## Era 3

```text
Many Computers

↓

Networks
```

---

## Era 4

```text
Internet
```

---

## Era 5

```text
Cloud Computing
```

---

## Era 6

```text
Containers

↓

Kubernetes
```

---

# Linux Networking Evolution

```text
Simple Ethernet

↓

TCP/IP

↓

Routing

↓

Firewalls

↓

Virtualization

↓

Namespaces

↓

Containers

↓

Orchestration

↓

Cloud Native
```

---

# Internals Thinking

Every modern networking technology still follows old principles.

## Principle 1

```text
Small pieces of data travel
```

(Packet Switching)

---

## Principle 2

```text
Standard communication rules
```

(Protocols)

---

## Principle 3

```text
Unique identities exist
```

(IP Addressing)

---

## Principle 4

```text
Routes determine paths
```

(Routing)

---

## Principle 5

```text
Networks should survive failures
```

(Decentralization)

---

# Real World Systems Using These Principles

| System | Uses |
|--------|------|
| AWS | TCP/IP |
| Azure | TCP/IP |
| Google Cloud | TCP/IP |
| Docker | Linux namespaces |
| Kubernetes | Virtual networking |
| Netflix | Microservices |
| YouTube | Distributed networking |
| GitHub | Global networking |

---

# Linux Engineer Mindset

Do not memorize protocols.

Ask:

```text
What problem was solved?

↓

Why was it invented?

↓

What replaced older systems?

↓

How does Linux implement it?

↓

How is it used today?
```

---

# WH Questions

## Why was networking invented?

Resource sharing.

---

## Why was packet switching invented?

Efficient communication.

---

## Why was TCP/IP invented?

To connect different networks.

---

## Why was DNS invented?

Humans cannot remember IP addresses.

---

## Why was routing invented?

Packets need paths.

---

## Why did Linux dominate networking?

Open source, stable, scalable.

---

# Key Takeaways

Modern networking is built on a few simple ideas.

```text
Communication

↓

Packet Switching

↓

Protocols

↓

Addressing

↓

Routing

↓

Standardization

↓

Scalability

↓

Linux Infrastructure
```

---

# Next File

```text
networking-terminologies.md
```

You will learn all the fundamental networking terms used everywhere.
