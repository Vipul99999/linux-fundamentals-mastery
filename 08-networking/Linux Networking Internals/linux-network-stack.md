# Linux Network Stack

# From Application → NIC → Wire → Internet → Back Again

---

# Why this file exists

Most Linux networking tutorials teach commands.

Few teach:

> **How Linux actually moves packets inside the operating system.**

If you understand this file, you'll understand:

* Why packets disappear
* Why services become unreachable
* Why latency happens
* Why Kubernetes networking works
* Why Docker networking works
* How firewalls work
* How load balancers work
* How proxies work
* How cloud networking works

This file is the foundation for:

* Backend Engineers
* DevOps Engineers
* SREs
* Cloud Engineers
* Security Engineers

---

# The Big Picture

Linux networking is essentially a huge packet processing pipeline.

Everything revolves around one object:

```text
Packet
```

Packets continuously move through Linux.

Linux decides:

```text
Receive packet
↓

Inspect packet

↓

Modify packet

↓

Route packet

↓

Filter packet

↓

Deliver packet

↓

Send packet
```

---

# Mental Model

Think of Linux as a giant airport.

```text
Application = Passenger

Socket = Ticket counter

TCP/UDP = Flight manager

IP Layer = Airport routing system

Firewall = Security check

Routing Table = Departure board

NIC = Airplane

Internet = Sky
```

Packet movement:

```text
Application

↓

Socket

↓

Transport Layer

↓

Network Layer

↓

Firewall

↓

Network Interface Card

↓

Wire

↓

Internet
```

---

# OSI vs Linux Reality

People often memorize OSI.

Engineers think differently.

---

# Textbook OSI

```text
7 Application

6 Presentation

5 Session

4 Transport

3 Network

2 Data Link

1 Physical
```

---

# Linux Practical View

Linux simplifies this.

```text
Application

↓

Socket API

↓

TCP / UDP

↓

IP

↓

Netfilter

↓

Qdisc (Traffic Control)

↓

Device Driver

↓

NIC

↓

Wire
```

This is what actually matters.

---

# Complete Linux Packet Journey

```text
Application

↓

Socket()

↓

TCP/UDP

↓

IP

↓

Routing Table

↓

Netfilter

↓

Traffic Control

↓

NIC Driver

↓

NIC Hardware

↓

Physical Wire

↓

Router

↓

Internet

↓

Destination Host

↓

Destination Linux Stack

↓

Application
```

---

# Linux Networking Architecture

```text
+------------------------+
|      Application       |
+------------------------+

           |

           v

+------------------------+
|        Socket API      |
+------------------------+

           |

           v

+------------------------+
|      TCP / UDP         |
+------------------------+

           |

           v

+------------------------+
|        IP Layer        |
+------------------------+

           |

           v

+------------------------+
|    Routing Decision    |
+------------------------+

           |

           v

+------------------------+
|      Netfilter         |
| iptables / nftables    |
+------------------------+

           |

           v

+------------------------+
| Traffic Control (tc)   |
+------------------------+

           |

           v

+------------------------+
|     Device Driver      |
+------------------------+

           |

           v

+------------------------+
|          NIC           |
+------------------------+

           |

           v

      Physical Wire
```

---

# Layer 1: Application Layer

Applications don't understand Ethernet.

Applications don't understand routers.

Applications simply say:

```text
I want to send data.
```

Examples:

```text
curl

nginx

postgres

redis

nodejs

python

java

ssh

docker
```

Example:

```python
import socket

s=socket.socket()

s.connect(("google.com",80))

s.send(b"hello")
```

Application never touches hardware.

Linux handles everything.

---

# Layer 2: Socket API

Sockets are the bridge.

Application ↔ Kernel

Think:

```text
Application Door

↓

Socket

↓

Kernel Network Stack
```

System calls:

```c
socket()

bind()

listen()

accept()

connect()

send()

recv()

close()
```

Example:

```text
Browser

↓

socket()

↓

Kernel
```

---

# Layer 3: TCP / UDP Layer

Responsible for communication rules.

TCP responsibilities:

```text
Reliable delivery

Ordering

Flow control

Congestion control

Retransmission

Connection management
```

UDP responsibilities:

```text
Fast

Connectionless

No guarantees
```

---

# TCP Visualization

```text
Sender

Packet1

Packet2

Packet3

↓

Receiver

ACK1

ACK2

ACK3
```

---

# TCP Handshake

```text
Client

SYN

↓

Server

SYN ACK

↓

Client

ACK
```

Connection established.

---

# Layer 4: IP Layer

IP answers:

```text
Where should packet go?
```

IP doesn't care about reliability.

Only addressing.

Packet contains:

```text
Source IP

Destination IP

TTL

Protocol

Checksum
```

Example:

```text
192.168.1.10

↓

8.8.8.8
```

---

# Layer 5: Routing Decision

Linux consults routing table.

View:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0

10.0.0.0/24 dev eth1

172.16.0.0/16 dev docker0
```

Decision flow:

```text
Packet arrives

↓

Destination IP

↓

Search routing table

↓

Longest prefix match

↓

Choose interface
```

---

# Longest Prefix Match

Linux always chooses most specific route.

Example:

```text
10.0.0.0/8

10.1.0.0/16

10.1.1.0/24
```

Destination:

```text
10.1.1.25
```

Linux chooses:

```text
10.1.1.0/24
```

---

# Layer 6: Netfilter (Firewall Engine)

Netfilter intercepts packets.

Tools:

```text
iptables

nftables

ufw

firewalld
```

Operations:

```text
Accept

Drop

Reject

Modify

NAT
```

Visualization:

```text
Packet

↓

Firewall

↓

Allowed ?

↓

Yes → Continue

No → Drop
```

---

# Netfilter Hooks

Linux packet interception points:

```text
PREROUTING

INPUT

FORWARD

OUTPUT

POSTROUTING
```

Visualization:

```text
Incoming

↓

PREROUTING

↓

INPUT

↓

Application



Outgoing

↓

OUTPUT

↓

POSTROUTING

↓

NIC
```

---

# Layer 7: Traffic Control (tc)

Controls traffic behavior.

Features:

```text
Bandwidth limits

Delay

Priority

Packet shaping

Queue management
```

Example:

```bash
tc qdisc show
```

Use cases:

```text
Video traffic priority

API rate shaping

Traffic simulation
```

---

# Layer 8: Device Driver

Drivers translate kernel instructions.

```text
Kernel Language

↓

Driver

↓

Hardware Language
```

Examples:

```text
e1000

ixgbe

virtio_net

mlx5
```

View:

```bash
ethtool -i eth0
```

---

# Layer 9: NIC

NIC = Network Interface Card

Responsibilities:

```text
Transmit packets

Receive packets

Checksum offloading

Segmentation offloading

Interrupt handling
```

Examples:

```text
eth0

ens33

eno1

enp0s3
```

View:

```bash
ip link
```

---

# Receive Packet Journey (RX Path)

Incoming traffic:

```text
Wire

↓

NIC

↓

Driver

↓

Kernel

↓

Netfilter

↓

Routing

↓

TCP/UDP

↓

Socket

↓

Application
```

---

# Send Packet Journey (TX Path)

Outgoing traffic:

```text
Application

↓

Socket

↓

TCP/UDP

↓

IP

↓

Routing

↓

Netfilter

↓

Traffic Control

↓

Driver

↓

NIC

↓

Wire
```

---

# Real Example: Browser Opening Google

```text
Chrome

↓

DNS Lookup

↓

Socket

↓

TCP Handshake

↓

TLS Handshake

↓

HTTP Request

↓

Packet Transmission

↓

Internet

↓

Google Server

↓

Response

↓

Browser Render
```

Thousands of packets participate.

---

# Linux Networking in Docker

Docker adds layers.

```text
Container

↓

veth

↓

docker0 bridge

↓

iptables NAT

↓

Host NIC

↓

Internet
```

---

# Linux Networking in Kubernetes

Kubernetes expands further.

```text
Pod

↓

veth

↓

CNI

↓

Bridge

↓

iptables

↓

Host NIC

↓

Internet
```

---

# Modern Cloud Example

EC2 VM sending packet.

```text
Application

↓

Linux Stack

↓

eth0

↓

Hypervisor

↓

Virtual Switch

↓

AWS Network Fabric

↓

Internet Gateway

↓

Internet
```

Cloud is still Linux networking underneath.

---

# Where Do Engineers Debug?

Production debugging usually occurs here:

```text
Application

Socket

TCP

Routing

Firewall

NIC
```

Not everywhere.

---

# Troubleshooting Ladder

```text
Application working?

↓

Socket open?

↓

DNS working?

↓

TCP handshake working?

↓

Route exists?

↓

Firewall blocking?

↓

NIC healthy?

↓

Physical network healthy?
```

---

# Essential Commands Map

| Goal           | Command          |
| -------------- | ---------------- |
| Interfaces     | ip link          |
| IP addresses   | ip addr          |
| Routes         | ip route         |
| Sockets        | ss               |
| Packets        | tcpdump          |
| Firewall       | nft list ruleset |
| Driver         | ethtool          |
| Statistics     | ip -s link       |
| Neighbor table | ip neigh         |

---

# Production Troubleshooting Flow

```text
Service unreachable

↓

DNS ?

↓

Socket Listening ?

↓

TCP Handshake ?

↓

Route Exists ?

↓

Firewall Blocking ?

↓

NAT Issue ?

↓

NIC Healthy ?

↓

External Network ?
```

---

# Common Misconceptions

## Misconception 1

> Linux networking = IP addresses

Wrong.

IP addresses are a tiny part.

Networking includes:

```text
Sockets

TCP

UDP

Routing

Firewall

Queues

Drivers

NIC

Namespaces

Virtual Networking
```

---

## Misconception 2

> Docker networking is separate networking

Wrong.

Docker uses Linux networking primitives.

---

## Misconception 3

> Kubernetes invented networking

Wrong.

Kubernetes orchestrates Linux networking.

---

## Misconception 4

> Firewall only blocks traffic

Wrong.

Firewall can:

```text
Filter

Modify

NAT

Track connections
```

---

# Engineer Mental Model

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

Socket

↓

TCP/UDP

↓

IP

↓

Route

↓

Firewall

↓

Queue

↓

Driver

↓

NIC

↓

Network

↓

NIC

↓

Driver

↓

Firewall

↓

TCP/UDP

↓

Socket

↓

Application
```

This is how Linux engineers think.

---

# Capability Checklist

After this file you should be able to answer:

✅ How packets move inside Linux

✅ Difference between OSI and Linux networking

✅ Linux network stack architecture

✅ Packet TX journey

✅ Packet RX journey

✅ Role of sockets

✅ Role of routing

✅ Role of netfilter

✅ Role of traffic control

✅ Role of NIC

✅ Docker networking foundations

✅ Kubernetes networking foundations

✅ Production debugging flow


These files will progressively build production-level Linux networking expertise.
