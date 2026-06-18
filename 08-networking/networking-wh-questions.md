# Networking WH Questions

> If you can answer these questions without memorization, you truly understand networking.

---

# How To Use This File

Do not simply read the answers.

For every question:

1. Think for 30 seconds.
2. Draw a diagram.
3. Explain it aloud.
4. Then verify your answer.

---

# Networking Mental Model

```text
Application

↓

Protocol

↓

Packet

↓

IP Address

↓

Router

↓

Internet

↓

Router

↓

Destination

↓

Application
```

---

# Section 1: Big Picture Networking

## What is networking?

## Why do computers need networking?

## Why can't every application communicate directly?

## How do two computers communicate?

## How does data travel across the world?

## What problems does networking solve?

## Why was TCP/IP created?

## Why does the internet work?

## Why is networking layered?

## What would happen if networking layers didn't exist?

---

# Section 2: Client Server Model

```text
Client

↓

Request

↓

Server

↓

Response

↓

Client
```

Questions:

### What is a client?

### What is a server?

### Why do we separate clients and servers?

### How many clients can connect to one server?

### What happens if millions of clients connect?

### Why do servers need ports?

### How does a client know where the server is?

### What happens if a server goes down?

### Why are APIs considered client-server communication?

### Why is a browser a client?

---

# Section 3: OSI Model

```text
Application

Presentation

Session

Transport

Network

Data Link

Physical
```

Questions:

### What is the OSI model?

### Why was the OSI model created?

### Why are there seven layers?

### Why don't applications directly send electrical signals?

### What responsibility belongs to each layer?

### Why does each layer exist?

### Which layers do developers mostly work with?

### Which layers do network engineers mostly work with?

### Which layers does Linux interact with?

### Why is the OSI model still taught?

---

# Section 4: TCP/IP Model

```text
Application

Transport

Internet

Network Access
```

Questions:

### What is TCP/IP?

### Why was TCP/IP created?

### Why is TCP/IP used instead of OSI?

### How is TCP/IP different from OSI?

### What protocols belong to each layer?

### Why is TCP/IP the internet standard?

### What would happen without TCP/IP?

---

# Section 5: IP Addressing

Questions:

### What is an IP address?

### Why do devices need IP addresses?

### Why can't devices communicate using names?

### What is IPv4?

### What is IPv6?

### Why are IPv4 addresses running out?

### Why was IPv6 created?

### Why are there private IP addresses?

### Why are there public IP addresses?

### How are IP addresses assigned?

### Who assigns IP addresses?

### What is DHCP?

### Why do devices receive different IP addresses?

---

# Section 6: Ports

Questions:

### What is a port?

### Why do ports exist?

### Why isn't an IP address enough?

### How many ports exist?

### Why is port 80 important?

### Why is port 443 important?

### Why is port 22 important?

### Why are some ports reserved?

### How does Linux know which application owns a port?

### What happens if two applications use the same port?

---

# Section 7: DNS

Visual:

```text
google.com

↓

DNS

↓

142.x.x.x

↓

Server
```

Questions:

### What is DNS?

### Why was DNS created?

### Why can't humans remember IP addresses?

### How does DNS work?

### Where does DNS lookup start?

### Why is DNS hierarchical?

### What is a DNS resolver?

### What is a root DNS server?

### What is a TLD server?

### What is an authoritative DNS server?

### Why is DNS caching important?

### What happens if DNS fails?

---

# Section 8: TCP

Questions:

### What is TCP?

### Why was TCP created?

### Why is TCP reliable?

### How does TCP guarantee delivery?

### What is a sequence number?

### What is an acknowledgment?

### Why does TCP retransmit packets?

### Why does TCP use buffers?

### What is flow control?

### What is congestion control?

### Why is TCP slower than UDP?

---

# Section 9: TCP Handshake

```text
Client            Server

SYN ------------>

<------ SYN ACK

ACK ------------>
```

Questions:

### Why does TCP use a three-way handshake?

### Why isn't one handshake enough?

### Why isn't two handshakes enough?

### What information is exchanged?

### Why does synchronization matter?

### What would happen without the handshake?

### What is SYN flooding?

---

# Section 10: UDP

Questions:

### What is UDP?

### Why was UDP created?

### Why is UDP faster than TCP?

### Why is UDP unreliable?

### Why is reliability sometimes unnecessary?

### When should we use UDP?

### Why do games use UDP?

### Why does video streaming use UDP?

### Why does DNS use UDP?

---

# Section 11: HTTP & HTTPS

Questions:

### What is HTTP?

### Why was HTTP created?

### Why is HTTP stateless?

### Why is statelessness important?

### What is HTTPS?

### Why was HTTPS created?

### Why is encryption important?

### How does HTTPS work?

### What is TLS?

### Why do websites use HTTPS?

---

# Section 12: Routing

```text
PC

↓

Router1

↓

Router2

↓

Router3

↓

Server
```

Questions:

### What is routing?

### Why do routers exist?

### How does a router make decisions?

### What is a routing table?

### Why are multiple routers needed?

### How does internet routing work?

### Why isn't the internet one giant switch?

---

# Section 13: NAT

Questions:

### What is NAT?

### Why was NAT invented?

### Why do home networks use NAT?

### Why are private IP addresses useful?

### How does NAT conserve IPv4 addresses?

### What problems does NAT create?

### Why do games sometimes struggle with NAT?

---

# Section 14: Firewalls

Questions:

### What is a firewall?

### Why do firewalls exist?

### What problems do they solve?

### How do firewalls filter traffic?

### Why are firewalls essential for servers?

### What is stateful filtering?

### What is stateless filtering?

---

# Section 15: Linux Networking

Questions:

### Why does Linux dominate networking?

### Why do cloud providers use Linux?

### How does Linux handle packets?

### What is the Linux network stack?

### What are sockets?

### Why are sockets important?

### How does Linux process incoming packets?

### How does Linux process outgoing packets?

### What is a network interface?

### What is a virtual network interface?

---

# Section 16: Containers

Questions:

### Why do containers need networking?

### How do containers communicate?

### What is a Docker bridge?

### Why do containers have separate IPs?

### Why do containers use namespaces?

### What is container isolation?

---

# Section 17: Kubernetes

Questions:

### Why does Kubernetes need networking?

### Why does every pod have an IP?

### How do pods communicate?

### What is a service?

### Why does Kubernetes abstract networking?

### How does service discovery work?

---

# Section 18: Cloud Networking

Questions:

### What is VPC?

### Why do clouds create private networks?

### What is a subnet?

### What is an internet gateway?

### What is a NAT gateway?

### Why do cloud providers abstract networking?

---

# First-Principles Questions (Most Important)

These are expert-level thinking questions.

### Why doesn't the internet collapse?

### Why is networking layered?

### Why don't routers become bottlenecks?

### Why is reliability difficult?

### Why do we need protocols?

### Why do packets exist?

### Why don't we send entire files at once?

### Why is latency unavoidable?

### Why is bandwidth limited?

### Why is DNS one of the most critical systems?

### Why are distributed systems hard?

### Why is networking considered one of the hardest areas in engineering?

---

# Mastery Checklist

If you can explain these without memorizing:

```text
✅ Client Server

✅ OSI

✅ TCP/IP

✅ DNS

✅ TCP

✅ UDP

✅ HTTP

✅ HTTPS

✅ Routing

✅ NAT

✅ Firewall

✅ Linux Networking

✅ Docker Networking

✅ Kubernetes Networking

✅ Cloud Networking
```

Then your networking fundamentals are strong.
tions.md
```
