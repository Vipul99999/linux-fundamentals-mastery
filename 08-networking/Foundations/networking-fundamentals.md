# Networking Fundamentals

> Learn how computers communicate, how data travels between systems, and how Linux participates in networking.

---

# What Is Networking?

Networking is the process of connecting multiple devices so they can exchange information and share resources.

Those devices can be:

- Computers
- Servers
- Smartphones
- Routers
- Switches
- Printers
- IoT devices
- Containers
- Virtual Machines
- Cloud servers

Networking allows these devices to communicate with each other.

Without networking:

- Websites cannot exist
- Cloud cannot exist
- SSH cannot work
- APIs cannot communicate
- Databases cannot be accessed remotely
- Distributed systems cannot exist

---

# Simple Definition

> Networking is the language computers use to communicate.

Humans use languages.

Computers use:

- Protocols
- Addresses
- Packets
- Routes

---

# Why Do We Need Networking?

Imagine 1000 computers.

Without networking:

```text
Computer A ❌ Cannot communicate with Computer B

Computer B ❌ Cannot access data from Computer C

Computer C ❌ Cannot access the internet
```

Networking solves this problem.

```text
Computer A

↓

Network

↓

Computer B

↓

Internet
```

---

# Real World Examples

You use networking every day.

## Example 1: Opening YouTube

```text
Laptop

↓

WiFi Router

↓

ISP

↓

Internet

↓

YouTube Server

↓

Video returns
```

---

## Example 2: Sending WhatsApp Message

```text
Your Phone

↓

Internet

↓

WhatsApp Servers

↓

Friend's Phone
```

---

## Example 3: SSH Into Linux Server

```text
Your Laptop

↓

Internet

↓

Linux Server

↓

Command Execution

↓

Result Returns
```

---

# Core Building Blocks Of Networking

Networking is built from several components.

```text
Device

↓

Address

↓

Protocol

↓

Packet

↓

Router

↓

Destination
```

---

# Networking Components Overview

| Component | Purpose |
|-----------|---------|
| Device | Sends or receives data |
| Network Interface | Connects device to network |
| Address | Identifies device |
| Protocol | Communication rules |
| Packet | Unit of transferred data |
| Router | Forwards packets |
| Switch | Connects local devices |
| DNS | Converts names to IP addresses |
| Gateway | Entry/Exit point to another network |

---

# How Networking Actually Works

Suppose you visit:

```text
https://google.com
```

A lot happens.

```text
You

↓

Browser

↓

DNS lookup

↓

Get Google's IP

↓

TCP connection

↓

TLS encryption

↓

HTTP request

↓

Google server

↓

Response returns

↓

Browser renders page
```

All of this happens within milliseconds.

---

# Networking Is A Journey

Networking is simply moving data from one place to another.

```text
Source

↓

Communication rules

↓

Travel path

↓

Destination
```

Example:

```text
Laptop

↓

WiFi

↓

Router

↓

ISP

↓

Internet

↓

Google Server
```

---

# Fundamental Questions Networking Solves

Every network must answer these questions.

## 1. Who am I?

Identity.

Answer:

IP Address

MAC Address

---

## 2. Where should data go?

Destination.

Answer:

IP Address

---

## 3. How do I reach it?

Path finding.

Answer:

Routing

---

## 4. How do we communicate?

Communication rules.

Answer:

Protocols

---

## 5. Is data arriving correctly?

Reliability.

Answer:

TCP

---

## 6. How do humans identify systems?

Naming.

Answer:

DNS

---

# Networking Is Like Postal Delivery

Think of networking as a postal system.

| Real World | Networking |
|------------|-----------|
| House | Device |
| House Address | IP Address |
| Person Name | Domain Name |
| Parcel | Packet |
| Roads | Network |
| Traffic Rules | Protocols |
| Post Office | Router |
| GPS | Routing |

---

# Data Travels In Small Pieces

Computers do not send huge files all at once.

Example:

```text
100 MB File
```

Becomes:

```text
Packet 1

Packet 2

Packet 3

Packet 4

...

Packet N
```

Destination reconstructs them.

---

# Fundamental Networking Concepts

These concepts appear everywhere.

| Concept | Meaning |
|---------|---------|
| Host | Device connected to network |
| Client | Requests services |
| Server | Provides services |
| Packet | Small unit of data |
| Protocol | Communication rules |
| Port | Doorway to application |
| Interface | Connection point |
| Address | Device identity |
| Route | Path to destination |
| Gateway | Exit point |

---

# Types Of Networks

## PAN (Personal Area Network)

Very small network.

Examples:

```text
Phone

↓

Bluetooth Earbuds
```

Range:

```text
1m - 10m
```

---

## LAN (Local Area Network)

Inside buildings.

Examples:

```text
Office

Home

School
```

---

## MAN (Metropolitan Area Network)

Entire city.

Examples:

```text
City-wide infrastructure
```

---

## WAN (Wide Area Network)

Large geographical areas.

Examples:

```text
Countries

Continents

Internet
```

---

# Network Communication Types

## One To One

```text
Computer A

↓

Computer B
```

---

## One To Many

```text
Server

↙ ↓ ↘

Users
```

---

## Many To Many

```text
Users

↓

Servers

↓

Databases

↓

Services
```

Modern systems use this.

---

# Client Server Model

Most networking uses this model.

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

Examples:

| Client | Server |
|--------|--------|
| Browser | Website |
| Mobile App | API |
| SSH Client | Linux Server |

---

# Peer To Peer Model

Devices communicate directly.

```text
Device A

↔

Device B
```

Examples:

- Torrent
- Blockchain
- Local file sharing

---

# Modern World Networking

Networking is everywhere.

## Cloud

```text
Users

↓

Load Balancer

↓

Servers

↓

Databases
```

---

## Microservices

```text
Frontend

↓

API Gateway

↓

Service A

↓

Service B

↓

Database
```

---

## Containers

```text
Container A

↓

Virtual Network

↓

Container B
```

---

## Kubernetes

```text
Pods

↓

Services

↓

Ingress

↓

Users
```

---

# Linux Perspective

Linux powers much of the internet.

Linux is used in:

- Servers
- Cloud
- Kubernetes
- Supercomputers
- IoT devices
- Routers
- Datacenters

Linux is responsible for:

- Packet processing
- Routing
- Firewalls
- Sockets
- DNS requests
- Network security

Linux networking mostly happens inside the kernel.

---

# Linux Networking Components

```text
Application

↓

Socket API

↓

Linux Kernel

↓

Network Stack

↓

NIC (Network Interface Card)

↓

Physical Network
```

---

# Networking Mindset

Whenever networking fails, always think:

```text
WHO?

↓

WHERE?

↓

HOW?

↓

WHAT FAILED?

↓

WHY FAILED?
```

---

# Real World Troubleshooting Flow

Suppose:

```text
Website is not opening.
```

Never guess.

Follow this.

```text
1. Is internet connected?

↓

2. Is DNS working?

↓

3. Is destination reachable?

↓

4. Is routing correct?

↓

5. Is firewall blocking?

↓

6. Is server alive?

↓

7. Is application running?
```

This mindset is used by Linux engineers.

---

# Skills You Will Build In This Folder

After completing this networking section, you will be able to:

✅ Understand packet flow

✅ Understand Linux networking internals

✅ Troubleshoot systems

✅ Configure networking

✅ Understand cloud communication

✅ Understand container networking

✅ Understand distributed systems

✅ Analyze network traffic

---

# WH Questions

## What is networking?

Connecting devices to exchange information.

---

## Why do we need networking?

To allow systems to communicate and share resources.

---

## How do computers communicate?

Using protocols.

---

## Why do packets exist?

Large data is split into smaller transferable units.

---

## Why do protocols exist?

To create standardized communication rules.

---

## Why do routers exist?

To move packets between networks.

---

## Why does DNS exist?

Humans remember names better than IP addresses.

---

## Why is networking important for Linux?

Linux powers most modern infrastructure.

---

# Key Takeaways

Networking is not about commands.

Networking is understanding communication.

Always think:

```text
Who is talking?

↓

Who is receiving?

↓

What rules are being used?

↓

How is data travelling?

↓

How does Linux process it?

↓

What can fail?

↓

How do we troubleshoot it?
```

---

# What's Next?

Next file:

```text
networking-history.md
```

You will learn:

- How networking evolved
- Why protocols were invented
- How the internet was created
- How Linux became dominant in networking infrastructure
