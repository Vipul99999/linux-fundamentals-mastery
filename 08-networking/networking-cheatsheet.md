# Networking Cheatsheet

> A quick reference guide for Linux networking, computer networking fundamentals, troubleshooting, and production systems.

---

# Networking Mental Model

```text
Application

↓

Protocol

↓

Port

↓

Socket

↓

Packet

↓

IP Address

↓

Router

↓

Internet

↓

Destination
```

---

# Network Communication Flow

```text
Browser

↓

DNS

↓

IP Address

↓

TCP Connection

↓

HTTP Request

↓

Server

↓

HTTP Response
```

---

# OSI Model

```text
7. Application

6. Presentation

5. Session

4. Transport

3. Network

2. Data Link

1. Physical
```

Memory Trick:

```text
All
People
Seem
To
Need
Data
Processing
```

---

# TCP/IP Model

```text
Application

Transport

Internet

Network Access
```

---

# OSI vs TCP/IP

| OSI          | TCP/IP         |
| ------------ | -------------- |
| Application  | Application    |
| Presentation | Application    |
| Session      | Application    |
| Transport    | Transport      |
| Network      | Internet       |
| Data Link    | Network Access |
| Physical     | Network Access |

---

# Important Protocols

| Protocol   | Port  | Purpose           |
| ---------- | ----- | ----------------- |
| HTTP       | 80    | Websites          |
| HTTPS      | 443   | Secure Websites   |
| SSH        | 22    | Remote Login      |
| FTP        | 21    | File Transfer     |
| DNS        | 53    | Domain Resolution |
| DHCP       | 67/68 | IP Assignment     |
| SMTP       | 25    | Email Sending     |
| POP3       | 110   | Email Receiving   |
| IMAP       | 143   | Email Access      |
| MySQL      | 3306  | Database          |
| PostgreSQL | 5432  | Database          |
| Redis      | 6379  | Cache             |
| MongoDB    | 27017 | Database          |

---

# TCP vs UDP

| Feature        | TCP | UDP     |
| -------------- | --- | ------- |
| Reliable       | ✅   | ❌       |
| Fast           | ❌   | ✅       |
| Ordered        | ✅   | ❌       |
| Connection     | Yes | No      |
| Error Checking | Yes | Minimal |

TCP Uses:

```text
HTTPS

SSH

FTP

Databases
```

UDP Uses:

```text
Gaming

Video Calls

Streaming

DNS
```

---

# TCP Handshake

```text
Client            Server

SYN ------------>

<------ SYN ACK

ACK ------------>
```

Connection Established.

---

# TCP Termination

```text
Client            Server

FIN ------------>

<-------- ACK

<-------- FIN

ACK ------------>
```

Connection Closed.

---

# DNS Flow

```text
google.com

↓

DNS Resolver

↓

Root DNS

↓

TLD Server

↓

Authoritative DNS

↓

IP Address

↓

Server
```

---

# Packet Journey

```text
Laptop

↓

Router

↓

ISP

↓

Internet

↓

Datacenter

↓

Server
```

---

# Network Devices

| Device        | Purpose               |
| ------------- | --------------------- |
| Hub           | Broadcast everything  |
| Switch        | Connect local devices |
| Router        | Connect networks      |
| Firewall      | Filter traffic        |
| Load Balancer | Distribute traffic    |
| Modem         | ISP communication     |
| Access Point  | WiFi access           |

---

# IP Address Basics

IPv4:

```text
192.168.1.10
```

IPv6:

```text
2001:db8::1
```

---

# Private IPv4 Ranges

Class A:

```text
10.0.0.0/8
```

Class B:

```text
172.16.0.0/12
```

Class C:

```text
192.168.0.0/16
```

---

# CIDR Quick Reference

| CIDR | Hosts |
| ---- | ----- |
| /32  | 1     |
| /30  | 4     |
| /29  | 8     |
| /28  | 16    |
| /27  | 32    |
| /26  | 64    |
| /25  | 128   |
| /24  | 256   |

---

# Important Linux Networking Commands

## Show IP Address

```bash
ip a
```

---

## Show Interfaces

```bash
ip link
```

---

## Show Routing Table

```bash
ip route
```

---

## Show Listening Ports

```bash
ss -tuln
```

---

## Ping Host

```bash
ping google.com
```

---

## DNS Lookup

```bash
dig google.com
```

or

```bash
nslookup google.com
```

---

## Trace Network Path

```bash
traceroute google.com
```

---

## Test HTTP Request

```bash
curl https://google.com
```

---

## Download File

```bash
wget https://example.com/file.zip
```

---

## Test SSH

```bash
ssh user@server_ip
```

---

## Transfer File

```bash
scp file.txt user@server:/home/user/
```

---

# Most Important `ss` Commands

Listening Ports:

```bash
ss -tuln
```

Listening Process Names:

```bash
ss -tulpn
```

Established Connections:

```bash
ss -tan
```

---

# Most Important `netstat` Commands

```bash
netstat -tuln

netstat -rn

netstat -an
```

---

# Important `tcpdump` Commands

Capture Everything:

```bash
sudo tcpdump
```

Specific Interface:

```bash
sudo tcpdump -i eth0
```

Port Specific:

```bash
sudo tcpdump port 80
```

Host Specific:

```bash
sudo tcpdump host 8.8.8.8
```

---

# Important `nmap` Commands

Basic Scan:

```bash
nmap 192.168.1.1
```

Port Scan:

```bash
nmap -p 1-1000 192.168.1.1
```

Service Detection:

```bash
nmap -sV 192.168.1.1
```

OS Detection:

```bash
nmap -O 192.168.1.1
```

---

# Network Troubleshooting Flow

```text
Application Problem?

↓

DNS Working?

↓

Ping Working?

↓

Port Open?

↓

Route Correct?

↓

Firewall Blocking?

↓

Server Healthy?
```

---

# DNS Troubleshooting

Check:

```bash
dig google.com

nslookup google.com

cat /etc/resolv.conf
```

---

# Connectivity Troubleshooting

Layer 1

```text
Cable

WiFi

Power
```

↓

Layer 2

```text
NIC

MAC

Switch
```

↓

Layer 3

```text
IP

Route

Gateway
```

↓

Layer 4

```text
Port

TCP

UDP
```

↓

Layer 7

```text
Application
```

---

# Linux Network Files

Hosts File:

```text
/etc/hosts
```

DNS Config:

```text
/etc/resolv.conf
```

Hostname:

```text
/etc/hostname
```

Network Config (varies):

```text
/etc/network/

/etc/netplan/

/etc/sysconfig/network-scripts/
```

---

# Docker Networking

```text
Container

↓

Docker Bridge

↓

Host

↓

Internet
```

---

# Kubernetes Networking

```text
User

↓

Service

↓

Pod

↓

Container
```

---

# Reverse Proxy

```text
User

↓

Nginx

↓

Backend Services
```

---

# Load Balancer

```text
Users

↓

Load Balancer

↓

Server1

Server2

Server3
```

---

# Cloud Networking

```text
VPC

↓

Subnet

↓

Security Group

↓

Load Balancer

↓

Compute Instance
```

---

# Networking Golden Rules

```text
1. DNS before everything

2. IP before ports

3. Ports before applications

4. Firewalls break things often

5. Most problems are DNS, firewall, or configuration issues

6. Learn packet flow, not commands

7. Think layer by layer
```

---

# Networking Mastery Roadmap

```text
Client Server

↓

OSI

↓

TCP/IP

↓

IP Addressing

↓

Ports

↓

DNS

↓

TCP

↓

UDP

↓

HTTP

↓

HTTPS

↓

Routing

↓

NAT

↓

Firewall

↓

Linux Networking

↓

Docker Networking

↓

Kubernetes Networking

↓

Cloud Networking
```
