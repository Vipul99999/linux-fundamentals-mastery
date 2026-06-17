# Linux Network Namespaces

# How Linux Creates Multiple Independent Networks Inside One Machine

---

# Why This File Exists

Most people first encounter namespaces through Docker.

They hear:

> "Containers are isolated."

But **how?**

The answer is:

> Linux Namespaces.

Network namespaces are one of the most important Linux technologies ever created.

Without them, these modern systems would not exist:

* Docker
* Kubernetes
* Podman
* LXC
* Containerd
* CRI-O
* Service Meshes
* Cloud Native Platforms

If you understand network namespaces deeply, Docker and Kubernetes become much easier.

---

# The Big Idea

Normally a Linux machine has one network stack.

Example:

```text
Machine

eth0

192.168.1.100

Routing Table

ARP Table

Firewall Rules

Socket Table
```

Everything shares one network environment.

---

# Network Namespace Creates Multiple Independent Network Stacks

```text
Machine

┌─────────────────┐
│ Namespace A     │
│                 │
│ eth0            │
│ 10.0.0.2        │
│                 │
└─────────────────┘

┌─────────────────┐
│ Namespace B     │
│                 │
│ eth0            │
│ 10.0.0.3        │
│                 │
└─────────────────┘

┌─────────────────┐
│ Host Namespace  │
│                 │
│ eth0            │
│ 192.168.1.100   │
│                 │
└─────────────────┘
```

Every namespace believes:

```text
I own this network.
```

But Linux is virtualizing it.

---

# Mental Model

Think of namespaces as apartments inside a building.

```text
Building = Linux Host

Apartment = Namespace
```

Each apartment has its own:

```text
Doors

Windows

Water pipes

Electricity
```

Similarly:

Each namespace has its own:

```text
Interfaces

Routes

ARP cache

Firewall

Sockets
```

---

# What Is Actually Isolated?

Each namespace gets its own copy of:

```text
Network Interfaces

IP Addresses

Routing Tables

ARP Tables

Neighbor Tables

Firewall Rules

Socket Tables

Port Numbers

Connection Tracking
```

---

# Visualize It

Without namespaces:

```text
Applications

↓

Single Network Stack

↓

eth0

↓

Internet
```

With namespaces:

```text
App A

↓

Namespace A

↓

Virtual Interface

↓

Internet



App B

↓

Namespace B

↓

Virtual Interface

↓

Internet
```

---

# Namespace Hierarchy

```text
Linux Host

│

├── Host Namespace

│

├── Namespace A

│

├── Namespace B

│

└── Namespace C
```

---

# Why Containers Need This

Without namespaces:

```text
Container A

Port 80

Container B

Port 80

❌ Conflict
```

With namespaces:

```text
Container A

Namespace A

Port 80



Container B

Namespace B

Port 80

✅ No conflict
```

Both can run simultaneously.

---

# What Happens Internally?

When a namespace is created:

Linux creates a completely new network environment.

```text
Namespace

↓

New Routing Table

↓

New ARP Cache

↓

New Socket Space

↓

New Firewall Rules
```

But by default:

```text
No interfaces
```

except:

```text
lo
```

---

# Verify This

Create namespace:

```bash
sudo ip netns add dev
```

Check:

```bash
sudo ip netns list
```

Output:

```text
dev
```

---

# Enter Namespace

Execute commands inside it.

```bash
sudo ip netns exec dev bash
```

Now you're inside.

Check interfaces:

```bash
ip link
```

Output:

```text
lo
```

---

# Why Only Loopback?

Every namespace starts empty.

Linux doesn't automatically connect it.

You must build networking.

---

# Loopback Interface

Initially:

```text
lo

DOWN
```

Enable it:

```bash
ip link set lo up
```

Verify:

```bash
ip addr
```

---

# Important Concept

Namespaces don't magically connect.

They need a cable.

That cable is:

```text
veth
```

---

# veth Pair

veth = Virtual Ethernet Cable

Imagine this:

```text
End A <==============> End B
```

Anything entering one side exits the other side.

---

# Visualization

```text
Namespace A

veth0

<==========>

veth1

Host Namespace
```

---

# Creating veth Pair

```bash
sudo ip link add veth0 type veth peer name veth1
```

Linux creates:

```text
veth0

<======>

veth1
```

---

# Move Interface Into Namespace

```bash
sudo ip link set veth0 netns dev
```

Now:

```text
Namespace dev

veth0



Host Namespace

veth1
```

---

# Assign IP Addresses

Namespace:

```bash
sudo ip netns exec dev ip addr add 10.0.0.2/24 dev veth0
```

Host:

```bash
sudo ip addr add 10.0.0.1/24 dev veth1
```

Bring interfaces up:

```bash
sudo ip netns exec dev ip link set veth0 up

sudo ip link set veth1 up
```

---

# Final Architecture

```text
Host Namespace

10.0.0.1

veth1

<==============>

veth0

10.0.0.2

Namespace dev
```

---

# Test Connectivity

Ping host:

```bash
sudo ip netns exec dev ping 10.0.0.1
```

Works.

---

# Packet Journey

Namespace → Host

```text
Namespace App

↓

Socket

↓

TCP/IP

↓

veth0

↓

veth1

↓

Host Kernel

↓

Host Network Stack
```

---

# Namespace → Internet Journey

```text
Namespace

↓

veth

↓

Host

↓

iptables NAT

↓

eth0

↓

Internet
```

This is exactly how Docker works.

---

# Namespace Routing Tables

Each namespace has its own routing table.

Host:

```bash
ip route
```

Namespace:

```bash
sudo ip netns exec dev ip route
```

They are independent.

---

# Visual

```text
Host Route Table

192.168.1.0/24

default via 192.168.1.1



Namespace Route Table

10.0.0.0/24
```

Separate worlds.

---

# Namespace Firewall Rules

Every namespace has independent firewall rules.

Host:

```bash
sudo nft list ruleset
```

Namespace:

```bash
sudo ip netns exec dev nft list ruleset
```

Independent.

---

# Socket Isolation

Host:

```text
nginx

Port 80
```

Namespace:

```text
nginx

Port 80
```

No conflicts.

---

# How Docker Uses Namespaces

Docker internally does:

```text
Container

↓

Create Namespace

↓

Create veth

↓

Attach Bridge

↓

Assign IP

↓

Enable NAT
```

---

# Docker Architecture

```text
Container A

↓

eth0

↓

vethA

↓

docker0 Bridge

↓

Host eth0

↓

Internet
```

---

# Kubernetes Architecture

Kubernetes also uses namespaces.

```text
Pod

↓

Network Namespace

↓

veth

↓

CNI Plugin

↓

Bridge

↓

Host
```

---

# Every Pod Gets A Namespace

```text
Node

│

├── Pod A Namespace

├── Pod B Namespace

├── Pod C Namespace
```

---

# Production Example

Microservices:

```text
Auth Service

Payment Service

Notification Service
```

Without namespaces:

```text
Port conflicts
```

With namespaces:

```text
Isolated networks
```

Everything runs independently.

---

# Security Benefits

Namespaces provide isolation.

They reduce attack surface.

Without namespaces:

```text
Compromised process

↓

Entire host exposed
```

With namespaces:

```text
Compromised container

↓

Limited exposure
```

Important:

> Namespaces are not complete security boundaries.

Combine with:

```text
cgroups

Capabilities

Seccomp

AppArmor

SELinux
```

---

# Production Troubleshooting

---

## Problem 1

Container cannot access internet.

Check:

```bash
ip route
```

---

## Problem 2

Cannot ping host.

Check:

```bash
ip link
```

---

## Problem 3

DNS not working.

Check:

```bash
cat /etc/resolv.conf
```

inside namespace.

---

## Problem 4

Packets disappear.

Check:

```bash
nft list ruleset
```

---

# Namespace Debugging Flow

```text
Container Broken

↓

Namespace Exists?

↓

veth Exists?

↓

IP Assigned?

↓

Route Exists?

↓

Firewall Blocking?

↓

NAT Working?

↓

Host Internet Working?
```

---

# Essential Commands

Create namespace:

```bash
ip netns add dev
```

List:

```bash
ip netns list
```

Delete:

```bash
ip netns delete dev
```

Execute:

```bash
ip netns exec dev bash
```

Create veth:

```bash
ip link add veth0 type veth peer name veth1
```

Move interface:

```bash
ip link set veth0 netns dev
```

View routes:

```bash
ip route
```

---

# Common Misconceptions

## Misconception 1

> Namespaces are Docker features

Wrong.

Docker uses Linux namespaces.

---

## Misconception 2

> Containers are lightweight VMs

Wrong.

Containers are isolated Linux processes.

---

## Misconception 3

> Namespaces provide full security

Wrong.

They provide isolation.

Not complete security.

---

# Engineer Mental Model

Never think:

```text
Docker Container
```

Think:

```text
Process

↓

Namespace

↓

veth

↓

Bridge

↓

NAT

↓

Host Network
```

---

# Capability Checklist

After this file you should understand:

✅ Network namespaces

✅ Why Docker works

✅ Why Kubernetes works

✅ veth pairs

✅ Namespace routing

✅ Namespace firewalls

✅ Namespace packet journey

✅ Namespace isolation

✅ Container networking fundamentals

---

# Next File

```text
virtual-networking.md
```

That file will connect:

```text
Namespaces

↓

veth

↓

Bridges

↓

Virtual Switches

↓

NAT

↓

Internet
```

and complete the foundation for Docker and Kubernetes networking.
